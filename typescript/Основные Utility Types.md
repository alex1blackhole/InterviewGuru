https://piccalil.li/blog/real-world-uses-of-typescripts-utility-types/

В статье рассматриваются практические примеры использования **Utility Types** в TypeScript, которые помогают упростить работу с типами и сделать код более гибким.

#### 🔹 Основные Utility Types и их применение:


**`Partial<T>`** – делает все свойства типа `T` необязательными
```js
interface User {  
  id: number;  
  name: string;  
  email: string;  
}  

function updateUser(id: number, updates: Partial<User>) {  
  // Обновляем только переданные поля  
}  

updateUser(1, { name: "Alex" }); // OK, email не обязателен
```  

**`Required<T>`** - Делает все свойства обязательными (даже если в оригинале они `?`).

```js 
interface Config {  
  apiUrl?: string;  
  timeout?: number;  
}  

const strictConfig: Required<Config> = {  
  apiUrl: "https://api.example.com",  
  timeout: 5000,  
};  
// Ошибка, если не указать оба поля
```

**`Pick<T, K>`** – Выбирает только указанные свойства из типа `T`

```js
interface Post {  
  id: number;  
  title: string;  
  content: string;  
  createdAt: Date;  
}  

type PostPreview = Pick<Post, "id" | "title">;  
// { id: number; title: string }  

const preview: PostPreview = {  
  id: 1,  
  title: "Hello TS",  
};
```

**`Omit <T, K> `** -Исключает указанные свойства.

```js
interface User {  
  id: number;  
  name: string;  
  password: string;  
}  

type SafeUser = Omit<User, "password">;  
// { id: number; name: string }  

const publicUser: SafeUser = {  
  id: 1,  
  name: "Alex",  
};
```

**`Readonly<T>`** - запрещает изменение свойств


```js
interface AppConfig {  
  version: string;  
}  

const config: Readonly<AppConfig> = {  
  version: "1.0",  
};  

config.version = "2.0"; // Ошибка: нельзя изменять
```

**`Record <K, T>`** - Создает тип с ключами `K` и значениями `T`

```js
type PageViews = Record<"home" | "about" | "contact", number>;  

const views: PageViews = {  
  home: 1000,  
  about: 500,  
  contact: 200,  
};
```

 **`ReturnType<T>`** - Извлекает тип возвращаемого значения функции.

```js 
function fetchData() {  
  return { data: "some data", status: 200 };  
}  

type ApiResponse = ReturnType<typeof fetchData>;  
// { data: string; status: number }  

const response: ApiResponse = {  
  data: "test",  
  status: 200,  
};
```


**`Exclude<T, U> `**  - убирает из `Т` типы, которые есть в `U`
**`Extract<T, U>`**  - оставляет только общие типы

```js
type T = "a" | "b" | "c";  
type Excluded = Exclude<T, "a" | "b">; // "c"  
type Extracted = Extract<T, "a" | "d">; // "a"
```
