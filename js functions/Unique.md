
1. `new Set(...)` - создает коллекцию Set из элементов массива. Set автоматически удаляет дубликаты, так как может содержать только уникальные значения
    
2. `[...new Set(...)]` - преобразует Set обратно в массив с помощью оператора spread . . .

```js 
function unique(arr) {
  return [...new Set(arr.map(item => item))]
}

```

```js
unique(['twinkle', 'twinkle', 'little', 'bat']); 
// Результат: ['twinkle', 'little', 'bat']
```