
//вернуть массив с массивами чисел + N шаг
//получаем чанки [[1,2],[2,3]]

functiont(arr, n) {
    return arr.reduce((acc, cur, index) => {
				//Нужно проверить что можем взять N чисел
        // иначе уйдет лишний массив

				if(n > array.length) return  [arr]

        if (index + n <= arr.length) {
            return [...acc, arr.slice(index, index + n)];
        } else {
            return [...acc];
        }

    }, []);
}
