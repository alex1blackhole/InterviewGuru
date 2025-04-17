
**React.memo** — это Higher-Order Component (HOC) для мемоизации функциональных компонентов:


```js
const MemoizedComponent = React.memo(Component, [arePropsEqual?]);
```

### Как это работает:

1. **Запоминает** результат последнего рендера компонента
    
2. **При новых пропсах**:
    
    - Сравнивает их с предыдущими (по shallow compare или через кастомную функцию)
        
    - Пропускает ререндер, если пропсы не изменились
        

## 💡 **Основное применение**

### 1. Оптимизация "чистых" компонентов

```js
const UserCard = ({ name, avatar }) => (
  <div>
    <img src={avatar} alt={name} />
    <h3>{name}</h3>
  </div>
);

export default React.memo(UserCard); // Рендерится только при изменении пропсов
```

### 2. С кастомной функцией сравнения

```js
function areEqual(prevProps, nextProps) {
  // Рендерить только если изменился ID пользователя
  return prevProps.user.id === nextProps.user.id;
}

export default React.memo(UserProfile, areEqual);
```


## 🆚 **Сравнение с похожими техниками**

|Характеристика|React.memo|useMemo|PureComponent|
|---|---|---|---|
|**Тип**|HOC|Хук|Классовый компонент|
|**Оптимизирует**|Весь компонент|Отдельные значения|shouldComponentUpdate|
|**Для функц. компонентов**|Да|Да|Нет|

## ⚠️ **Распространенные ошибки**

### 1. Бесполезное использование с часто меняющимися пропсами


```js
// Плохо (пропс onClick создается заново при каждом рендере)
const Button = React.memo(({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
));
```

**Решение:** Обернуть функцию в useCallback

```js
const memoizedButton = React.memo(({ onClick, children }) => (...));

function Parent() {
  const handleClick = useCallback(() => {...}, []);
  return <MemoizedButton onClick={handleClick} />;
}
```

### 2. Мемоизация компонентов с children

```js
// Плохо (children всегда новый объект)
const Layout = React.memo(({ children }) => (
  <div className="layout">{children}</div>
));
```

**Решение:** Либо не мемоизировать, либо контролировать children

### 3. Излишняя оптимизация простых компонентов

```js
// Плохо (рендер дешевле, чем сравнение пропсов)
const SimpleText = React.memo(({ text }) => <span>{text}</span>);
```

### 4. Неправильная кастомная функция сравнения


```js
// Опасная практика (может пропускать важные обновления)
const UnsafeMemo = React.memo(Component, () => true);
```

## 🏆 **Правильные паттерны использования**

### 1. Оптимизация сложных списков

```js
const UserList = ({ users }) => (
  <ul>
    {users.map(user => (
      <MemoizedUserItem key={user.id} user={user} />
    ))}
  </ul>
);

const UserItem = React.memo(({ user }) => (...));
```

### 2. Компоненты с тяжелым рендером


```js
const HeavyChart = React.memo(({ data }) => {
  // Сложные вычисления и отрисовка графика
  return <svg>...</svg>;
});
```

### 3. Контролируемые пропсы

```js
const ControlledInput = React.memo(({ value, onChange }) => (
  <input value={value} onChange={onChange} />
));
```

## 📊 **Когда использовать React.memo?**

✔ **Компоненты с тяжелым рендером**  
✔ **Чистые презентационные компоненты**  
✔ **Элементы списков с стабильными пропсами**  
✔ **Когда пропсы меняются редко**

❌ **Не используйте для:**

- Компонентов, которые всегда получают новые пропсы
    
- Очень простых компонентов
    
- Компонентов с часто меняющимися children
    

## 🚀 **Вывод**

**React.memo** — мощный инструмент оптимизации, но требует осмысленного применения:

1. **Измеряйте производительность** перед оптимизацией
    
2. **Комбинируйте** с useMemo/useCallback для сложных случаев
    
3. **Избегайте** преждевременной оптимизации простых компонентов

```js
// Идеальный кандидат для React.memo
const UserProfile = React.memo(({ user }) => (
  <div>
    <Avatar url={user.avatar} />
    <UserInfo data={user.info} />
  </div>
));
```