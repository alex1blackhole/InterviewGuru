**Event Bus** — это паттерн, позволяющий компонентам обмениваться событиями **без явной передачи пропсов** и без вложенности.

Когда:

- **Компоненты не связаны** (например, `Header` и `Footer`).
    
- **Глубокая вложенность** (проброс пропсов через 10 компонентов — плохо).
    
- **Микрофронтенды** или независимые модули.
    

---

## **🔧 Реализация Event Bus**

### **1. Простейшая реализация (на JavaScript)**

```js 
class EventBus {
  constructor() {
    this.listeners = {};  // { eventName: [callbacks...] }
  }

  on(event, callback) {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event].push(callback);
  }

  off(event, callback) {
    if (!this.listeners[event]) return;
    this.listeners[event] = this.listeners[event].filter(
      (listener) => listener !== callback
    );
  }

  emit(event, data) {
    if (!this.listeners[event]) return;
    this.listeners[event].forEach((listener) => listener(data));
  }
}

// Создаем глобальный экземпляр
const bus = new EventBus();
export default bus;
```

```js 
import bus from './eventBus';

function ComponentA() {
  const handleClick = () => {
    bus.emit('buttonClicked', { message: 'Hello from A!' });
  };

  return <button onClick={handleClick}>Click me</button>;
}
```


```js
import { useEffect } from 'react';
import bus from './eventBus';

function ComponentB() {
  useEffect(() => {
    const handler = (data) => {
      console.log('Event received:', data.message);
    };

    bus.on('buttonClicked', handler);
    return () => bus.off('buttonClicked', handler);  // Отписка
  }, []);

  return <div>Listening for events...</div>;
}
```

## **Альтернативы Event Bus**

| **Способ**              | **Плюсы**                           | **Минусы**                             |
| ----------------------- | ----------------------------------- | -------------------------------------- |
| **Event Bus**           | Простота, независимость компонентов | Глобальное состояние, сложнее дебагать |
| **Context API**         | Встроен в React                     | Проблемы с оптимизацией                |
| **Redux/MobX**          | Управление состоянием               | Оверкилл для простых задач             |
| **Custom Events (DOM)** | Нативно в браузере                  | Только для DOM-элементов               |

## **⚡️ Примеры использования**
### **1. Уведомления между компонентами**

```js
// Где-то в коде
bus.emit('showNotification', { text: 'Success!', type: 'success' });

// В компоненте Notifications
bus.on('showNotification', (data) => toast(data.text));
```
### **2. Синхронизация данных**

```js
// При изменении темы
bus.emit('themeChanged', { mode: 'dark' });

// В разных компонентах
bus.on('themeChanged', (theme) => applyTheme(theme));
```

### **3. Управление модальными окнами**

```js
// Открываем модалку из любого места
bus.emit('openModal', { id: 'user-settings' });

// Слушаем в корневом компоненте
bus.on('openModal', (data) => setActiveModal(data.id));
```


## **⚠️ Проблемы Event Bus**

1. **Утечки памяти** — если не отписываться от событий.
    
2. **Сложность отладки** — события «летают» по всему приложению.
    
3. **Конфликты имен** — если события называются одинаково.
    

**Как избежать:**  
✅ Всегда отписываться в `useEffect` (для React).  
✅ Использовать префиксы для событий (`user:updated`).  
✅ Логировать события в разработке.

---

## **📌 Вывод**

- **Event Bus** — это **глобальная шина событий** для связи компонентов.
    
- **Плюсы:** Простота, независимость от иерархии.
    
- **Минусы:** Сложность отладки, риск утечек.
    
- **Когда использовать:**
    
    - Для **несвязанных компонентов**.
        
    - В **микрофронтендах**.
        
    - Если **Redux/Context — избыточны**.

```js
// Лучшие практики:
bus.emit('user:updated', { id: 123 });  // Префиксы для пространств имен
bus.on('user:updated', (user) => { ... });
```

