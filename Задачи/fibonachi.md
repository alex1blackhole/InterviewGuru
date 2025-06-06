#JavaScript #Algorithms 

```js
# Функция вычисления чисел Фибоначчи �

```javascript
const fib = n => {
    // Базовые случаи: первые два числа равны 1
    if (n === 1 || n === 2) return 1;

    // Рекурсивный случай: F(n) = F(n-1) + F(n-2)
    return fib(n - 1) + fib(n - 2);
}
```

## 📝 Памятка:

1. **Эффективность**: Экспоненциальная сложность O(2ⁿ) - неэффективно для больших n
    
2. **Базовый случай**: Обязательное условие выхода из рекурсии
    
3. **Дублирование вычислений**: Функция многократно вычисляет одни и те же значения
    
4. **Глубина рекурсии**: Ограничена размером стека вызовов

## 🏻 Оптимизированные версии:

1. **Мемоизация** (кеширование результатов):

```js
const fibMemo = (n, memo = {}) => {
    if (n in memo) return memo[n];
    if (n <= 2) return 1;
    memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}
```

2. **Итеративный подход** (O(n) сложность):

```js
function fibIter(n) {
    let a = 1, b = 1;
    for (let i = 3; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}

```