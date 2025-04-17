## � **1. Алгоритм Diffing: как React сравнивает деревья**

**Что важно знать:**

- React использует **O(n)** алгоритм (а не O(n³) как точное сравнение)
    
- Два ключевых допущения:
    
    1. Элементы **разных типов** создают разные деревья
        
    2. Key помогают определить стабильные элементы между рендерами
        

**Пример для собеседования:**


```js
// До
<div>
  <Counter />
  <p>Text</p>
</div>

// После
<span>
  <Counter />
  <p>Text</p>
</span>
```

👉 **Что произойдет?**  
React уничтожит старое поддерево (`div` → `span`) и смонтирует новое, даже если дочерние элементы похожи!

## ⚡ **2. Продвинутые техники оптимизации**

### **a) Стабильные ссылки на компоненты**

```js
// ❌ Плохо (новая функция при каждом рендере)
<Child onClick={() => handleClick(id)} />

// ✅ Хорошо (стабильная ссылка)
const handleItemClick = useCallback((id) => () => handleClick(id), []);
<Child onClick={handleItemClick(id)} />
```

### **b) shouldComponentUpdate/PureComponent/memo**

```js
// Оптимизация для классов
class MyComp extends React.PureComponent {
  shouldComponentUpdate(nextProps) {
    // Кастомная логика сравнения
    return this.props.value !== nextProps.value;
  }
}

// Для функциональных компонентов
const MyComp = React.memo(({ value }) => {...}, (prev, next) => {
  return prev.value === next.value; // Пропускает рендер если true
});
```

### **c) Виртуализация списков**

```js
// react-window для огромных списков
import { FixedSizeList } from 'react-window';

<List height={600} itemCount={1000} itemSize={35}>
  {({ index, style }) => (
    <div style={style}>Row {index}</div>
  )}
</List>
```

## 🛠 **3. Опасные антипаттерны**

### **a) Мутации state**

```js
// ❌ Мутация напрямую
const [items, setItems] = useState([...]);
items.push(newItem); // Не вызовет ререндер
setItems(items); // React может пропустить обновление

// ✅ Иммутабельное обновление
setItems(prev => [...prev, newItem]);
```

### **b) Инлайн-стили и объекты**

```js
// ❌ Создает новый объект при каждом рендере
<div style={{ color: 'red' }} />

// ✅ Выносим в константы
const styles = { color: 'red' };
<div style={styles} />
```


## 🔍 **4. Глубокий анализ производительности**

### **a) Профилирование с React DevTools**

- Вкладка **Profiler** для записи рендеров
    
- **Highlight updates** для визуализации обновлений
    
- **Пламя рендеров** для анализа времени
    

### **b) Замеры в production**

```js
// Включить в production-сборке
import { unstable_trace as trace } from 'scheduler/tracing';

trace('Some event', performance.now(), () => {
  setState({...});
});
```


## 💡 **5. Продвинутые паттерны Reconciliation**

### **a) Ключи для принудительного ремаунта**

```js
// Сброс состояния при изменении userId
<UserProfile key={userId} />
```

### **b) Порталы и их влияние**

```js
// Порталы сохраняют контекст, но могут нарушать DOM-порядок
ReactDOM.createPortal(children, domNode)
```

### **c) Suspense и ленивая загрузка**

```js
// React может приостанавливать reconciliation
<Suspense fallback={<Spinner />}>
  <LazyComponent />
</Suspense>
```

## 🎯 **Что спрашивают на собеседованиях:**

1. **Как React определяет, что компонент нужно обновить?**
    
2. **Чем отличается Virtual DOM от Real DOM?**
    
3. **Как работают key в списках?**
    
4. **Как оптимизировать часто обновляемые списки?**
    
5. **Что делает shouldComponentUpdate и как его правильно использовать?**
    

## 📊 **Метрики производительности**

|Метрика|Хорошее значение|Плохое значение|
|---|---|---|
|Время рендера|< 16ms|> 50ms|
|Количество нод DOM|< 1k|> 10k|
|Глубина вложенности|< 5|> 10|

## 🛡 **Лучшие практики для продакшна:**

1. Используйте **React.memo** для "дорогих" компонентов
    
2. Избегайте **пропс-дриллинга** (используйте Context/State managers)
    
3. Для форм — **неуправляемые компоненты** (uncontrolled)
    
4. Разбивайте большие компоненты на **мелкие поддеревья**
    

## 🚀 **Вывод:**

Глубокое понимание Reconciliation позволяет:  
✔ Писать **высокопроизводительные** приложения  
✔ Избегать **ненужных ререндеров**  
✔ Эффективно **отлаживать** проблемы производительности  
✔ Давать **продвинутые ответы** на собеседованиях

Освойте эти техники — и ваш React-код выйдет на новый уровень!