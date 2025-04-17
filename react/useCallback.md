## 🔍 **Основы useCallback**

**useCallback** — это хук для мемоизации функций между рендерами:

```js
const memoizedCallback = useCallback(
  () => {
    // Функция
  },
  [dependencies], // Массив зависимостей
);
```

### Как это работает:

1. **При первом рендере** — создает и запоминает функцию
    
2. **При последующих рендерах**:
    
    - Возвращает ту же функцию, если зависимости не изменились
        
    - Создает новую функцию, если зависимости изменились
        

## 💡 **Основное применение**

### 1. Оптимизация дочерних компонентов (с React.memo)


```js
const Child = React.memo(({ onClick }) => {
  console.log('Render Child');
  return <button onClick={onClick}>Click</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  
  // Функция сохраняется между рендерами
  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);
  
  return (
    <>
      <Child onClick={handleClick} />
      <div>Count: {count}</div>
    </>
  );
}
```

### 2. Передача коллбеков в эффекты

```js
function Example({ id }) {
  const fetchData = useCallback(async () => {
    const data = await api.fetch(id);
    // ...
  }, [id]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);
}
```

## 🆚 **Сравнение с обычной функцией**

|Характеристика|useCallback|Обычная функция|
|---|---|---|
|**Создание**|Только при изменении зависимостей|Каждый рендер|
|**Ссылочная стабильность**|Сохраняется|Новая при каждом рендере|
|**Оптимизация**|Да|Нет|

## ⚠️ **Распространенные ошибки**

### 1. Излишнее использование

```js
// Плохо (простая функция не требует мемоизации)
const handleClick = useCallback(() => {
  console.log('Click');
}, []);
```

### 2. Пустой массив зависимостей

```js
// Ошибка: использует устаревшее состояние
const handleSubmit = useCallback(() => {
  console.log(value); // Всегда начальное значение value
}, []); // Нет зависимости value
```

### 3. Использование без React.memo

```js
// Бесполезно (дочерний компонент все равно перерендерится)
const Child = ({ onClick }) => <button onClick={onClick}>Click</button>;

function Parent() {
  const handleClick = useCallback(() => {...}, []);
  return <Child onClick={handleClick} />;
}
```

## 🏆 **Правильные паттерны использования**

### 1. Оптимизация списков

```js
const List = ({ items }) => {
  const renderItem = useCallback(
    (item) => <ListItem item={item} />,
    []
  );
  
  return <List items={items} renderItem={renderItem} />;
};
```

### 2. Передача в хук зависимости

```js
function useCustomHook(callback) {
  useEffect(() => {
    window.addEventListener('event', callback);
    return () => window.removeEventListener('event', callback);
  }, [callback]);
}
```

## 📊 **Когда использовать useCallback?**

✔ **Коллбеки в оптимизированных компонентах (React.memo)**  
✔ **Функции как зависимости эффектов**  
✔ **Функции, передаваемые в глубокие деревья компонентов**

❌ **Не используйте для:**

- Простых обработчиков
    
- Функций, которые не передаются в дочерние компоненты
    
- Когда нет измеримой проблемы производительности
    

## 🚀 **Вывод**

**useCallback** — это инструмент оптимизации:

1. **Используйте осознанно** — только при реальных проблемах производительности
    
2. **Комбинируйте с React.memo** для максимальной эффективности
    
3. **Правильно указывайте зависимости** — как в useEffect


```js
// Правильное использование
const memoizedHandler = useCallback(
  () => {
    // Логика
  },
  [dep1, dep2], // Явные зависимости
);
```

