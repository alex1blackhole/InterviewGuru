
#JavaScript #Algorithms #objects #recursion 

## 🔍 Базовая версия (без рекурсии)

```js
function isEqual1(object1, object2) {
    const props1 = Object.getOwnPropertyNames(object1);
    const props2 = Object.getOwnPropertyNames(object2);

    // Проверка количества свойств
    if (props1.length !== props2.length) {
        return false;
    }

    // Поверхностное сравнение значений
    for (let i = 0; i < props1.length; i++) {
        const prop = props1[i];
        if (object1[prop] !== object2[prop]) {
            return false;
        }
    }

    return true;
}
```

## 🔄 Рекурсивная версия (с вложенными объектами)

```js
function isEqual(object1, object2) {
    const props1 = Object.getOwnPropertyNames(object1);
    const props2 = Object.getOwnPropertyNames(object2);

    // Проверка количества свойств
    if (props1.length !== props2.length) {
        return false;
    }

    for (let i = 0; i < props1.length; i++) {
        const prop = props1[i];
        const bothAreObjects = typeof object1[prop] === 'object' && typeof object2[prop] === 'object';

        // Сравнение примитивов
        if (!bothAreObjects && object1[prop] !== object2[prop]) {
            return false;
        }

        // Рекурсивное сравнение объектов
        if (bothAreObjects && !isEqual(object1[prop], object2[prop])) {
            return false;
        }
    }

    return true;
}
```


## 📝 Памятка

1. **Проверка количества свойств** - сначала сравниваем количество ключей
    
2. **Типы значений** - определяем, являются ли значения объектами
    
3. **Примитивы** - сравниваем напрямую через `!==`

4. **Объекты** - рекурсивный вызов `isEqual`
    
5. **Edge cases**:
    
    - `null` имеет тип `'object'` (нужна дополнительная проверка)
        
    - Массивы требуют особого сравнения
        
    - Циклические ссылки могут вызвать переполнение стека