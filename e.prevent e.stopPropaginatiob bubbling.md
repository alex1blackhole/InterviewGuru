
### ✋ **`preventDefault()`**

**Назначение:**  
Отменяет стандартное поведение браузера, связанное с событием.

**Примеры использования:**

- Отмена отправки формы (`submit`).
    
- Запрет перехода по ссылке (`click` на `<a>`).
    
- Блокировка прокрутки колесом мыши (`wheel`).
    

**Пример кода:**

```ts
document.querySelector('a').addEventListener('click', (e) => {
  e.preventDefault(); // ссылка не перейдёт по href
  console.log('Клик по ссылке, но без перехода!');
});
```

### 🛑 **`stopPropagation()`**

**Назначение:**  
Останавливает **всплытие (bubbling)** события, чтобы оно не достигало родительских элементов.

**Пример:**

```js
<div class="parent">
  <button class="child">Кнопка</button>
</div>
```

```js 
document.querySelector('.child').addEventListener('click', (e) => {
  e.stopPropagation(); // событие не дойдёт до .parent
  console.log('Клик по кнопке, но родитель не узнает!');
});

document.querySelector('.parent').addEventListener('click', () => {
  console.log('Этот код не выполнится из-за stopPropagation()');
});
```

**Важно:**

- Не путать с **`stopImmediatePropagation()`** (останавливает и другие обработчики **на этом же элементе**).
    
- Не влияет на **погружение (capturing phase)**, только на всплытие.
    

---

### 🌊 **Всплытие событий (Event Bubbling)**

**Как работает:**

1. Событие сначала срабатывает на целевом элементе.
    
2. Затем поднимается (всплывает) по DOM-дереву до `window`.
    

**Пример:**

```js 
<div class="grandparent">
  <div class="parent">
    <div class="child"></div>
  </div>
</div>
```

```js
const elements = document.querySelectorAll('div');
elements.forEach(el => {
  el.addEventListener('click', () => console.log(`Клик по ${el.className}`));
});

// Клик по .child выведет:
// "Клик по child" → "Клик по parent" → "Клик по grandparent"
```

**Зачем нужно всплытие?**

- Позволяет использовать **делегирование событий** (один обработчик на родителе вместо множества на детях).
    

---

### 🎯 **Делегирование событий**

**Суть:** Обработка событий на родителе, а не на каждом дочернем элементе.

**Пример:**

```js
<ul class="menu">
  <li data-action="save">Сохранить</li>
  <li data-action="load">Загрузить</li>
</ul>
```

```js
document.querySelector('.menu').addEventListener('click', (e) => {
  if (e.target.dataset.action) {
    console.log(`Выбрано действие: ${e.target.dataset.action}`);
  }
});
```

**Плюсы:**

- Экономит память (меньше обработчиков).
    
- Работает для динамически добавленных элементов.
    

---

### ⚡ **Итог**

|Метод / Термин|Описание|
|---|---|
|`preventDefault()`|Отменяет действие браузера по умолчанию (например, переход по ссылке).|
|`stopPropagation()`|Останавливает всплытие события (но не действие по умолчанию).|
|**Всплытие (Bubbling)**|Событие поднимается от целевого элемента к `window`.|
|**Делегирование**|Обработка событий на родителе через `e.target`.|

**Совет:** Для полного контроля над событиями можно использовать **третью фазу** — **перехват (capturing)**, добавив `{ capture: true }` в `addEventListener`.