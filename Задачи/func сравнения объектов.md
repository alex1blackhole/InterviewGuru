functionisEqual1(object1, object2) {
    const props1 = Object.getOwnPropertyNames(object1);
    const props2 = Object.getOwnPropertyNames(object2);

    if (props1.length !== props2.length) {
        return false;
    }

    for (let i = 0; i < props1.length; i += 1) {
        const prop = props1[i];

        if (object1[prop] !== object2[prop]) {
            return false;
        }
    }

    return true;
}

с вложенными объектов через рекурсию

functionisEqual(object1, object2) {
    const props1 = Object.getOwnPropertyNames(object1);
    const props2 = Object.getOwnPropertyNames(object2);

    if (props1.length !== props2.length) {
        return false;
    }

    for (let i = 0; i < props1.length; i += 1) {
        const prop = props1[i];
        const bothAreObjects = typeof(object1[prop]) === 'object' && typeof(object2[prop]) === 'object';

        if(!bothAreObjects && (object1[prop] !== object2[prop])) {
            return false;
        }

        if(bothAreObjects && !isEqual(object1[prop], object2[prop])) {
            return false;
        }

    }

    return true;
}