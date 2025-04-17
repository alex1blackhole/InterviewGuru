
**useEffect** — это хук для работы с побочными эффектами в функциональных компонентах React. Он выполняется асинхронно после завершения рендеринга и отрисовки (paint) в браузере.

```js
useEffect(() => {
  // Код эффекта
  return () => {
    // Функция очистки
  };
}, [dependencies]);
```

## 💡 **Ключевые особенности**

1. **Асинхронное выполнение** — не блокирует отрисовку
    
2. **Выполняется после** commit фазы рендеринга
    
3. **Аналог в классах** — `componentDidMount`, `componentDidUpdate` и `componentWillUnmount` вместе взятые
    

## 🆚 **useEffect vs useLayoutEffect**

|Характеристика|useEffect|useLayoutEffect|
|---|---|---|
|**Время выполнения**|После отрисовки|До отрисовки|
|**Блокировка UI**|Нет|Да|
|**Использование**|99% случаев|Специфичные DOM-операции|
|**SSR**|Без проблем|Предупреждение|

## ⚠️ **Типичные проблемы и решения**

### 1. Мерцание при изменениях DOM

```js
// Проблема: пользователь может увидеть промежуточное состояние
useEffect(() => {
  if (ref.current) {
    ref.current.style.color = 'red'; // Видно мерцание
  }
}, []);
```
**Решение:** Используйте `useLayoutEffect` для синхронных визуальных изменений


### 2. Бесконечные циклы

```js
// Ошибка: эффект вызывает обновление состояния, которое триггерит эффект
useEffect(() => {
  setCount(count + 1); // Бесконечный цикл без зависимостей
}, [count]); // Тоже проблема, если setCount меняет count
```
**Решение:** Правильно указывайте зависимости или используйте функциональную форму `setCount`

### 3. Утечки памяти

```js
// Ошибка: не отписываемся от событий
useEffect(() => {
  window.addEventListener('resize', handleResize);
}, []);
```
**Решение:** Всегда возвращайте функцию очистки

```js
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

## 🏆 **Правильные паттерны использования**

### 1. Запросы данных

```js
useEffect(() => {
  let ignore = false;
  
  async function fetchData() {
    const result = await api.fetch(id);
    if (!ignore) setData(result);
  }

  fetchData();
  
  return () => { ignore = true; };
}, [id]);
```

### 2. Подписки на события

```js
useEffect(() => {
  const handler = () => console.log('Clicked!');
  document.addEventListener('click', handler);
  
  return () => document.removeEventListener('click', handler);
}, []);
```

### 3. Интеграция с сторонними библиотеками

```js
useEffect(() => {
  const chart = new ChartJS(ref.current, config);
  
  return () => chart.destroy();
}, [data]);
```

## 📊 **Когда использовать useEffect?**

✔ **Запросы данных**  
✔ **Установка подписок**  
✔ **Ручное изменение DOM** (когда не критично мерцание)  
✔ **Запуск анимаций**  
✔ **Логирование**

## 🚀 **Вывод**

**useEffect** — основной инструмент для побочных эффектов:

1. **По умолчанию** используйте `useEffect`
    
2. **Для визуальных изменений** — оцените необходимость `useLayoutEffect`
    
3. **Всегда** очищайте ресурсы
    
4. **Правильно** указывайте зависимости

```js
// Идеальное использование
useEffect(() => {
  // Эффект
  return () => {
    // Очистка
  };
}, [dep1, dep2]); // Явные зависимости
```