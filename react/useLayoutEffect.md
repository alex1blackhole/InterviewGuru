
**useLayoutEffect** — это хук, который запускается синхронно после обновления DOM, но до того как браузер выполнит отрисовку (paint).

```js
useLayoutEffect(() => {
  // Код эффекта
  return () => {
    // Очистка эффекта
  };
}, [dependencies]);
```

### Ключевые особенности:

- **Синхронное выполнение** — блокирует отрисовку браузера до завершения
    
- **Фаза выполнения** — между обновлением DOM и его отрисовкой
    
- **Аналог в классах** — `componentDidMount` и `componentDidUpdate`
    

## 💡 **Основные сценарии использования**

### 1. Измерение DOM-элементов

```js
function Tooltip({ children }) {
  const ref = useRef(null);
  const [width, setWidth] = useState(0);

  useLayoutEffect(() => {
    if (ref.current) {
      // Получаем точные размеры до отрисовки
      setWidth(ref.current.offsetWidth);
    }
  }, []);

  return <div ref={ref}>{children} (Width: {width}px)</div>;
}
```

### 2. Анимации и переходы

```js
function FadeIn({ children }) {
  const ref = useRef(null);

  useLayoutEffect(() => {
    const node = ref.current;
    node.style.transition = 'opacity 300ms';
    node.style.opacity = '0';
    
    // Синхронное изменение перед отрисовкой
    requestAnimationFrame(() => {
      node.style.opacity = '1';
    });
  }, []);

  return <div ref={ref}>{children}</div>;
}
```

### 3. Предотвращение "мигания" контента


```js
function Modal({ isOpen }) {
  const ref = useRef(null);

  useLayoutEffect(() => {
    if (isOpen) {
      // Синхронное позиционирование
      positionModal(ref.current);
    }
  }, [isOpen]);

  return isOpen ? <div ref={ref}>Modal Content</div> : null;
}
```

## 🆚 **Сравнение useLayoutEffect и useEffect**

|Характеристика|useLayoutEffect|useEffect|
|---|---|---|
|**Время выполнения**|До отрисовки|После отрисовки|
|**Блокировка**|Блокирует отрисовку|Не блокирует|
|**Использование**|Измерения, анимации|Запросы данных, подписки|
|**Серверный рендеринг**|Предупреждение|Без проблем|

## ⚠️ **Распространенные ошибки**

### 1. Излишнее использование

```js
// Плохо (замедляет отрисовку без необходимости)
useLayoutEffect(() => {
  document.title = 'New Title';
}, []);
```
**Решение:** Используйте обычный `useEffect` для таких задач

### 2. Долгие вычисления

```js
// Опасно (блокирует интерфейс)
useLayoutEffect(() => {
  performHeavyCalculation(); // Может заморозить интерфейс
}, []);
```
**Решение:** Выносите тяжелые операции в `useEffect` или Web Workers

### 3. Бесконечные циклы

```js
// Может привести к зависанию
useLayoutEffect(() => {
  setState(newValue); // Если триггерит ререндер
});
```


**Решение:** Всегда указывайте зависимости

## 🏆 **Продвинутые примеры**

### 1. Автоподстройка высоты textarea

```js
function AutoTextarea() {
  const ref = useRef(null);
  
  useLayoutEffect(() => {
    const textarea = ref.current;
    textarea.style.height = 'auto';
    textarea.style.height = `${textarea.scrollHeight}px`;
  }, [value]);

  return <textarea ref={ref} value={value} onChange={...} />;
}
```

### 2. Синхронная подгрузка стилей

```js
function ThemedComponent() {
  useLayoutEffect(() => {
    const style = document.createElement('style');
    style.innerHTML = `.theme { color: red }`;
    document.head.appendChild(style);
    
    return () => document.head.removeChild(style);
  }, []);
}
```

## 📊 **Когда использовать useLayoutEffect?**

✔ **Измерение DOM-элементов**  
✔ **Синхронные визуальные изменения**  
✔ **Предотвращение визуального "мигания"**  
✔ **Анимации, требующие точного времени**

❌ **Не используйте для:**

- Запросов данных
    
- Подписок на события
    
- Любых асинхронных операций
    
- Когда можно использовать useEffect
    

## 🚀 **Вывод**

**useLayoutEffect** — это мощный инструмент для работы с DOM перед отрисовкой:

1. **Используйте осторожно** — может блокировать отрисовку
    
2. **Применяйте только когда** нужны синхронные DOM-изменения
    
3. **По умолчанию** предпочитайте обычный `useEffect`

```js
// Правильное использование
useLayoutEffect(() => {
  const width = element.current.offsetWidth;
  // Используем width до того как пользователь увидит контент
}, [deps]);
```