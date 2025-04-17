## 🔍 **Основы useRef**

**Теги:** `React`, `Хуки`, `DOM`, `Производительность`, `Ссылки`

**useRef** — это хук React, который возвращает изменяемый объект с свойством `.current`. У него два основных применения:

1. **Доступ к DOM-элементам**
    
2. **Хранение изменяемых значений без перерисовки компонента**

```js
const refContainer = useRef(initialValue);
// refContainer.current доступен для чтения/записи
```

## 💡 **1. Доступ к DOM-элементам (основное использование)**

### 📌 Пример: Фокус на input при клике


```js
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  
  const onButtonClick = () => {
    // `current` указывает на смонтированный input-элемент
    inputEl.current.focus();
  };
  
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Фокус на поле</button>
    </>
  );
}
```

### 🎯 Когда использовать:

- Управление фокусом, выделение текста
    
- Интеграция со сторонними DOM-библиотеками
    
- Измерение размеров/положения элементов
    

## 💡 **2. Хранение изменяемых значений (без перерисовки)**

### 📌 Пример: Счетчик рендеров


```js
function RenderCounter() {
  const renderCount = useRef(0);
  
  useEffect(() => {
    renderCount.current = renderCount.current + 1;
  });
  
  return (
    <div>
      Рендеров: {renderCount.current}
    </div>
  );
}
```

### 📌 Пример: Хранение предыдущего значения

```js
function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = useRef();
  
  useEffect(() => {
    prevCount.current = count;
  }, [count]);
  
  return (
    <div>
      Сейчас: {count}, До этого: {prevCount.current}
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}
```


## 🔥 **Ключевые особенности useRef**

1. **Не вызывает ререндер** при изменении `.current`
    
2. **Значение сохраняется** между рендерами
    
3. **Можно изменять** `.current` в любое время
    
4. **Работает аналогично** instance-переменным в классах
    

## ⚠️ **Важные предупреждения**


```js
// ❌ Нельзя использовать ref в рендере
<div>{myRef.current}</div>

// ✅ Изменяйте refs только в эффектах или обработчиках
useEffect(() => {
  myRef.current = newValue;
}, [deps]);
```


## 🆚 **Сравнение с useState**

|Характеристика|useRef|useState|
|---|---|---|
|**Вызов ререндера**|Нет|Да|
|**Использование**|DOM, мутируемые значения|Состояние компонента|
|**Изменение значения**|Прямое (`.current`)|Через set-функцию|

## 🏆 **Продвинутые примеры**

### 1. Интеграция с Canvas


```js
function CanvasDraw() {
  const canvasRef = useRef(null);
  
  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    // Рисуем что-то...
  }, []);
  
  return <canvas ref={canvasRef} />;
}
```


### 2. Замер времени анимации

```js
function Animation() {
  const startTime = useRef(null);
  const frameRef = useRef(null);
  
  const animate = (time) => {
    if (!startTime.current) startTime.current = time;
    const progress = time - startTime.current;
    // Анимация...
    frameRef.current = requestAnimationFrame(animate);
  };
  
  useEffect(() => {
    frameRef.current = requestAnimationFrame(animate);
    return () => cancelAnimationFrame(frameRef.current);
  }, []);
}
```

## 🚀 **Вывод**

**useRef** — это мощный инструмент для:

1. **Доступа к DOM** (когда нужен прямой доступ к элементам)
    
2. **Хранения мутабельных значений** (когда изменения не должны вызывать ререндер)
    

Используйте его, когда:

- Нужно работать с DOM API напрямую
    
- Требуется сохранять данные между рендерами без перерисовки
    
- Нужен аналог instance-переменных из классовых компонентов
