```ts
export function isObject(object: any) {
  return Object.prototype.toString.call(object) === '[object Object]';
}

export function isArray(array: unknown) {
  return Object.prototype.toString.call(array) === '[object Array]';
}

function isTheSameType(item1: any, item2: any) {
  return (
    Object.prototype.toString.call(item1) ===
    Object.prototype.toString.call(item2)
  );
}

/**
 * ф-я для сравнения двух объектов или миссивов
 *
 * @paramobj1
*
 * @paramobj2
*/

function deepCompare(obj1: any, obj2: any): boolean {
  if (obj1 === obj2) {
    return true;
  }

  if (
    (!isObject(obj1) && !isArray(obj1)) ||
    (!isObject(obj2) && !isArray(obj2))
  ) {
    return false;
  }

  if (
    !isTheSameType(obj1, obj2) ||
    Object.keys(obj1).length !== Object.keys(obj2).length
  ) {
    return false;
  }

  for (let key of Object.keys(obj1)) {
    if (!obj2.hasOwnProperty(key)) {
      return false;
    }

    if (!deepCompare(obj1[key], obj2[key])) {
      return false;
    }
  }

  return true;
}

export defaultdeepCompare;
```

