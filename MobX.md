
MobX позволяет создавать множество независимых хранилищ (stores), которые могут существовать изолированно друг от друга или взаимодействовать при необходимости.

## Основные подходы к созданию сторов

### 1. Классовый подход (наиболее распространенный)

```js
import { makeAutoObservable } from 'mobx';

class CounterStore {
  count = 0;

  constructor() {
    makeAutoObservable(this);
  }

  increment() {
    this.count++;
  }

  decrement() {
    this.count--;
  }
}

// Создание нескольких независимых экземпляров
const counterStore1 = new CounterStore();
const counterStore2 = new CounterStore();
```

### 2. Функциональный подход (с использованием makeObservable)

```js 
import { makeObservable, observable, action } from 'mobx';

function createTodoStore() {
  return makeObservable({
    todos: [],
    addTodo(text) {
      this.todos.push({ text, completed: false });
    },
    toggleTodo(index) {
      this.todos[index].completed = !this.todos[index].completed;
    }
  }, {
    todos: observable,
    addTodo: action,
    toggleTodo: action
  });
}

// Создание нескольких независимых хранилищ
const todoStoreA = createTodoStore();
const todoStoreB = createTodoStore();
```

## Преимущества множества независимых сторов

1. **Изоляция состояния**: Каждый экземпляр хранилища содержит собственное состояние
    
2. **Масштабируемость**: Легко создавать новые экземпляры по мере необходимости
    
3. **Тестируемость**: Проще тестировать изолированные хранилища
    
4. **Гибкость**: Можно использовать разные экземпляры для разных частей приложения
    

## Пример использования нескольких сторов


```js 
// stores/counterStore.js
export class CounterStore {
  // ... реализация как выше ...
}

// stores/todoStore.js
export class TodoStore {
  // ... реализация как выше ...
}

// В компоненте React
import { observer } from 'mobx-react-lite';
import { CounterStore, TodoStore } from './stores';

const App = observer(() => {
  // Создаем несколько независимых экземпляров
  const counter1 = new CounterStore();
  const counter2 = new CounterStore();
  const todoList = new TodoStore();

  return (
    <div>
      <div>
        <h3>Counter 1: {counter1.count}</h3>
        <button onClick={() => counter1.increment()}>+</button>
      </div>
      <div>
        <h3>Counter 2: {counter2.count}</h3>
        <button onClick={() => counter2.increment()}>+</button>
      </div>
      <TodoListView store={todoList} />
    </div>
  );
});
```

## Когда использовать несколько сторов

1. Когда разные части приложения требуют одинаковой логики, но с разными данными
    
2. Для изолированных виджетов или компонентов
    
3. В тестировании для создания чистых экземпляров для каждого теста
    
4. При реализации многопользовательских интерфейсов (например, разные вкладки с одинаковыми компонентами)
    

MobX обеспечивает простой и эффективный способ управления множеством независимых состояний в вашем приложении.