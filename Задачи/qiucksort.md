
const arr = [0, 3, 2, 5, 6, 8, 1, 9, 4, 2, 1, 9, 6, 4, 1, -1, -5, 23, 6, 2, 35, 6, 3, 32]

let count = 0;

function quickSort(arr) {
    if (arr.length <= 1) return arr;

    let centerIndex = Math.floor(arr.length / 2)
    let centerEl = arr[centerIndex]


    let smallNumbers = [];
    let bigNumbers = [];


    for (let i = 0; i < arr.length; i++) {
        if (i === centerIndex) continue

        if (arr[i] < centerEl) {
            smallNumbers.push(arr[i])
        } else {
            bigNumbers.push(arr[i])
        }
    }

    return [...quickSort(smallNumbers), centerEl, ...quickSort(bigNumbers)]

}


console.log(quickSort(arr))