
 #EventLoop #JavaScript #mustHave #Promises
 

```js
console.log(1);

setTimeout(() => console.log(2));

Promise.resolve().then(() => console.log(3));

// таймер внутри исполнится позже
Promise.resolve().then(() =>setTimeout(() => console.log(4)));

Promise.resolve().then(() => console.log(5));

setTimeout(() => console.log(6));

console.log(7);

console.log(1)
```

## 📝 Памятка 

1. **Синхронный код** → выполняется сразу
    
2. **Микрозадачи (Promises, queueMicrotask)** → выполняются после синхронного кода
    
3. **Макрозадачи (setTimeout, setInterval)** → выполняются после всех микрозадач
    
4. **Новые задачи** → добавленные во время выполнения попадают в соответствующие очереди



## 🔍 Пошаговое объяснение:

1. **Синхронный код выполняется первым**:
    
    - `console.log(1)` → выводит `1`
        
    - `console.log(7)` → выводит `7`
        
2. **Микрозадачи (Promise) выполняются перед макрозадачами (setTimeout)**:
    
    - `Promise.resolve().then(() => console.log(3))` → выводит `3`
        
    - `Promise.resolve().then(() => console.log(5))` → выводит `5`
        
    - Второй промис добавляет новый таймаут:  
        `() => setTimeout(() => console.log(4))` → попадает в очередь макрозадач
        
3. **Макрозадачи (setTimeout) выполняются после микрозадач**:
    
    - `setTimeout(() => console.log(2))` → выводит `2`
        
    - `setTimeout(() => console.log(6))` → выводит `6`
        
    - Таймаут из промиса `() => console.log(4)` → выводит `4` (последним, так как был добавлен позже)

https://habr.com/ru/post/681882/
