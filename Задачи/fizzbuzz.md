
#JavaScript #Algorithms 

```js
const fizzBuzz = function(n) {
  let arr = [];

  for (let i = 1; i <= n; i++) {
    if (i % 15 === 0) {
      arr.push("FizzBuzz");
    } else if (i % 3 === 0) {
      arr.push("Fizz");
    } else if (i % 5 === 0) {
      arr.push("Buzz");
    } else {
      arr.push(String(i));
    }
  }

  return arr;
};
```


## 📝 Памятка

1. **Порядок проверок важен** - сначала проверяем делимость на 15
    
2. **Преобразование чисел в строки** - для согласованного вывода
    
3. **Оптимизация** - можно использовать тернарные операторы
    
4. **Альтернативный подход** - конкатенация строк "Fizz" и "Buzz"

```js
const fizzBuzz = n => 
  Array.from({length: n}, (_, i) => 
    (++i % 3 ? '' : 'Fizz') + (i % 5 ? '' : 'Buzz') || String(i)
  );
```