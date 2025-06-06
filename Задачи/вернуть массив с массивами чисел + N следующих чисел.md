#JavaScript #Algorithms


```js
/**
 * Функция разбивает массив на подмассивы заданного размера
 * @param {Array} arr - Исходный массив
 * @param {number} n - Размер чанков (шаг)
 * @returns {Array} - Массив с подмассивами
 */
function chunkArray(arr, n) {
    // 1. Проверка на пустой массив или невалидный шаг
    if (!arr.length || n <= 0) return [];
    
    // 2. Если шаг больше длины массива - возвращаем весь массив
    if (n >= arr.length) return [arr];
    
    // 3. Создаем чанки с помощью reduce
    return arr.reduce((acc, _, index) => {
        // 4. Проверяем, можем ли взть n элементов
        if (index + n <= arr.length) {
            return [...acc, arr.slice(index, index + n)];
        }
        return acc;
    }, []);
}

// Пример использования:
// console.log(chunkArray([1, 2, 3, 4], 2)); // [[1, 2], [2, 3], [3, 4]]
```


### **🔍Алгоритм**

1. **Проверка входных данных**
- Если массив пуст (`!arr.length`) или шаг `n <= 0` → возвращаем `[]`.

2. **Обработка случая, когда `n` больше длины массива**
- Если `n >= arr.length` → возвращаем `[arr]` (весь массив как один чанк).

3. **Разбиение массива на чанки**
- Используем `reduce` для итерации по массиву.
- На каждом шаге проверяем, можно ли взять `n` элементов (`index + n <= arr.length`).
- Если да → добавляем подмассив `arr.slice(index, index + n)` в аккумулятор.

4. **Возврат результата**
- Полученный массив чанков — финальный результат.