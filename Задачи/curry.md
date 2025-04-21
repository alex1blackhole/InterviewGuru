#JavaScript

"Каррирование — это преобразование функции от многих аргументов в набор функций, каждая из которых принимает один аргумент."
	
#### 1. Базовая версия

```js
/**
 * Каррирование для функций с двумя аргументами
 * @param {Function} f - Функция для каррирования
 * @returns {Function} - Каррированная функция
 */
function simpleCurry(f) {
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}
```

#### 2. Продвинутая версия

```js
/**
 * Универсальное каррирование для функций с N аргументами
 * @param {Function} func - Функция для каррирования
 * @returns {Function} - Каррированная функция
 */
function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function(...args2) {
        return curried.apply(this, args.concat(args2));
      };
    }
  };
}
```


**Пример использования**:

```js
function sum(a, b, c) { return a + b + c; }
const curriedSum = curry(sum);

curriedSum(1)(2)(3); // 6
curriedSum(1,2)(3);   // 6
```

