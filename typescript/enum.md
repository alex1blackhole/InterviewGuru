
**`enum` (перечисление)** — это конструкция, позволяющая определить набор именованных констант, которые можно использовать вместо "магических" чисел или строк.

#### **Особенности `enum` в TypeScript:**

1. **Типы значений**:
    
    - **Числовые (`numeric`)**:
```ts
enum Direction {
  Up = 1,    // Явное указание значения
  Down,      // 2 (автоинкремент)
  Left = 5,
  Right,     // 6
}
```

**Строковые (`string`)**:
```ts
enum Status {
  Success = "SUCCESS",
  Error = "ERROR",
}
```

**Гетерогенные (смешанные)**:

```ts
enum Mixed {
  Yes = 1,
  No = "NO",
}
```

**Доступ к значениям**:

```ts
console.log(Direction.Up); // 1
console.log(Status.Success); // "SUCCESS"
```

**Обратное маппирование** (только для числовых `enum`):

```js
console.log(Direction[1]); // "Up"
```

1. **Компиляция в JS**:
    
    - Enum превращается в объект с прямым и обратным маппированием (для числовых).
        
2. **Использование в типах**:

```ts
let dir: Direction = Direction.Up; // Тип — Direction
```

## **2. Продвинутый ответ (как работает под капотом + нюансы)**

### **Как компилируется `enum` в JavaScript?**

**Числовой enum**:

```ts
enum Direction { Up, Down }
```

↓ Компиляция ↓

```ts
var Direction;
(function (Direction) {
    Direction[Direction["Up"] = 0] = "Up";
    Direction[Direction["Down"] = 1] = "Down";
})(Direction || (Direction = {}));
```

**Что происходит**:

1. Создается объект `Direction`.
    
2. `Direction["Up"] = 0` → `Direction.Up === 0`.
    
3. `Direction[0] = "Up"` → обратное маппирование (`Direction[0] === "Up"`).
    

**Строковой enum**:

```ts
enum Status { Success = "SUCCESS" }
```

↓ Компиляция ↓

```ts
var Status;
(function (Status) {
    Status["Success"] = "SUCCESS";
})(Status || (Status = {}));
```

**Разница**:

- Нет обратного маппирования (только `Status.Success` → `"SUCCESS"`).
    

---

### **Особенности и подводные камни**

1. **Рекомендация: предпочитать `const enum`**
    
    - Оптимизирует производительность (значения подставляются напрямую при компиляции).
        
    - Не генерирует лишний JS-код.
      
```ts
const enum FastDirection { Up, Down }
console.log(FastDirection.Up); // В JS: console.log(0 /* Up */);
```

1. **Но**: нельзя использовать обратное маппирование.
    
2. **Ambient enums (для деклараций)**
    
    - Используются в `.d.ts` для описания существующих перечислений.
      
```ts
declare enum LibMode { Debug, Release }
```

**Тип `enum` — это объединение его значений**

```ts
enum UserRole { Admin = 1, User = 2 }
let role: UserRole = 3; // Ошибки нет! (но это может быть неожиданно)
```

**Как исправить**:

```ts
const enum StrictUserRole { Admin = 1, User = 2 }
let role: StrictUserRole = 3; // ❌ Ошибка TS!
```

1. **Enum vs Union Types**
    
    - **Enum** хорош для:
        
        - Читаемости кода.
            
        - Необходимости обратного маппирования.
            
    - **Union Types (`"A" | "B"`)** лучше, если:
        
        - Нужна простая типизация без рантайм-объекта.
            
        - Используются строковые литералы.
            

---

### **Примеры использования**

**1. Замена "магическим" числам**:

```ts
enum HttpCode { OK = 200, NotFound = 404 }
if (response.status === HttpCode.OK) { ... }
```

**2. Строковые enum для действий Redux**:

```ts
enum ActionType { Load = "LOAD_DATA", Save = "SAVE_DATA" }
dispatch({ type: ActionType.Load });
```

**3. Оптимизированный `const enum` для производительности**:

```ts
const enum LogLevel { Error, Warn, Info }
console.log(LogLevel.Error); // В JS: console.log(0);
```

### **Вывод**

- **Используйте `enum`** для удобства и читаемости.
    
- **`const enum`** — если не нужно обратное маппирование.
    
- **Union Types** — если достаточно простых литералов.
    
- **Избегайте гетерогенных `enum`** (усложняют поддержку).
