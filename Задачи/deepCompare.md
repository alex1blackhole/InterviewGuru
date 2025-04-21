#typescript #JavaScript #Algorithms

```typescript
# Глубокое сравнение объектов и массивов в TypeScript �🔍

## 📌 Утилиты для проверки типов

/**
 * Проверяет, является ли значение объектом
 * @param {any} object - Проверяемое значение
 * @returns {boolean}
 */
export function isObject(object: any): boolean {
  return Object.prototype.toString.call(object) === '[object Object]';
}

/**
 * Проверяет, является ли значение массивом
 * @param {unknown} array - Проверяемое значение
 * @returns {boolean}
 */
export function isArray(array: unknown): boolean {
  return Object.prototype.toString.call(array) === '[object Array]';
}

/**
 * Проверяет, имеют ли значения одинаковый тип
 * @param {any} item1 - Первое значение
 * @param {any} item2 - Второе значение
 * @returns {boolean}
 */
function isTheSameType(item1: any, item2: any): boolean {
  return (
    Object.prototype.toString.call(item1) ===
    Object.prototype.toString.call(item2)
  );
}

/**
 * Рекурсивно сравнивает два объекта или массива
 * @param {any} obj1 - Первый объект/массив
 * @param {any} obj2 - Второй объект/массив
 * @returns {boolean} - Результат сравнения
 */
function deepCompare(obj1: any, obj2: any): boolean {
  // 1. Проверка на строгое равенство
  if (obj1 === obj2) {
    return true;
  }

  // 2. Проверка что оба значения - объекты/массивы
  if ((!isObject(obj1) && !isArray(obj1)) || 
      (!isObject(obj2) && !isArray(obj2))) {
    return false;
  }

  // 3. Проверка на одинаковый тип и количество ключей
  if (!isTheSameType(obj1, obj2) || 
      Object.keys(obj1).length !== Object.keys(obj2).length) {
    return false;
  }

  // 4. Рекурсивное сравнение всех свойств
  for (let key of Object.keys(obj1)) {
    if (!obj2.hasOwnProperty(key)) {
      return false;
    }

    if (!deepCompare(obj1[key], obj2[key])) {
      return false;
    }
  }

  return true;
}

export default deepCompare;

```


## 📝 Памятка

1. **"Сначала проверяю строгое равенство"** - если объекты идентичны, возвращаем true
    
2. **"Проверяю что оба значения - объекты или массивы"** - используя isObject/isArray
    
3. **"Сравниваю типы и количество ключей"** - должны быть одинаковыми
    
4. **"Рекурсивно сравниваю все вложенные свойства"** - для каждого ключа рекурсивно вызываю deepCompare
    
5. **"Учитываю edge cases"**:
    
    - разные типы данных
        
    - отсутствие свойств
        
    - пустые объекты
        
    - циклические ссылки (в текущей реализации не обрабатываются)