
```ts
// Сохраняем оригинальный метод filter
const originalFilter = Array.prototype.filter;

// Переопределяем метод filter
Array.prototype.filter = function(callback, thisArg) {
  // Выводим исходный массив в консоль
  console.log('Исходный массив:', this);
  
  // Возвращаем исходный массив без фильтрации
  return [...this];
};

// Пример использования
const numbers = [1, 2, 3, 4, 5];
const filteredNumbers = numbers.filter(num => num > 3);

// В консоли будет:
// Исходный массив: [1, 2, 3, 4, 5]
// filteredNumbers содержит [1, 2, 3, 4, 5] (без фильтрации)
```

