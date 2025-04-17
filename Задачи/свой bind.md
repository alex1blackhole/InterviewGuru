const bindFn = function(fn,context, ...arguments) {
  return function(...args) {
	  return fn.apply(context,[...arguments, ...args])
	}
}

const bindFn2 = function (fn, context) {
    // обрезаем ненужные аргументы (функцию и контекст)
    let bindArgs = [].slice.call(arguments, 2);
    return function () {
        // здесь все аргументы будут необходимы
        let fnArgs = [].slice.call(arguments);
        // собираем все 
        return fn.apply(context, bindArgs.concat(fnArgs));
    };
};
