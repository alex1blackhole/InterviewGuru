#JavaScript #FunctionBinding

```js
/**
 * Реализация функции bind с использованием ES6+ синтаксиса
 * @param {Function} fn - Функция для привязки
 * @param {Object} context - Контекст выполнения
 * @param {...any} boundArgs - Аргументы для частичного применения
 * @returns {Function} - Новая функция с привязанным контекстом и аргументами
 */
const bindES6 = function(fn, context, ...boundArgs) {
  return function(...args) {
    return fn.apply(context, [...boundArgs, ...args]);
  }
};

/**
 * Классическая реализация функции bind (ES5-совместимая)
 * @param {Function} fn - Функция для привязки
 * @param {Object} context - Контекст выполнения
 * @returns {Function} - Новая функция с привязанным контекстом
 */
const bindES5 = function(fn, context) {
  // 1. Получаем дополнительные аргументы (кроме fn и context)
  const bindArgs = [].slice.call(arguments, 2);
  
  return function() {
    // 2. Получаем аргументы при вызове функции
    const callArgs = [].slice.call(arguments);
    
    // 3. Объединяем аргументы и вызываем оригинальную функцию
    return fn.apply(context, bindArgs.concat(callArgs));
  };
};

const user = {
  name: 'John',
  greet: function(greeting, punctuation) {
    return `${greeting}, ${this.name}${punctuation}`;
  }
};

const boundES6 = bindES6(user.greet, user, 'Hello');
console.log(boundES6('!')); // "Hello, John!"

const boundES5 = bindES5(user.greet, user, 'Hi');
console.log(boundES5('?')); // "Hi, John?"
```


