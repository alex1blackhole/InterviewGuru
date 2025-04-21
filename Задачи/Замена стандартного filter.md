#JavaScript #Arrays

```ts
/**
 * Переопределение стандартного метода Array.prototype.filter
 * для демонстрационных целей (отключает фильтрацию)
 */

// 1. Сохраняем оригинальный метод filter
const originalFilter = Array.prototype.filter;

// 2. Переопределяем метод filter
Array.prototype.filter = function(callback, thisArg) {
  // 2.1. Логируем исходный массив
  console.log('Исходный массив:', this);
  
  // 2.2. Возвращаем копию массива без фильтрации
  return [...this];
};

// 3. Пример использования
const numbers = [1, 2, 3, 4, 5];
const filteredNumbers = numbers.filter(num => num > 3);

// Результат:
// console: "Исходный массив: [1, 2, 3, 4, 5]"
// filteredNumbers: [1, 2, 3, 4, 5] (исходный массив без изменений)
```


