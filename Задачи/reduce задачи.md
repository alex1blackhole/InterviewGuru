function map(arr, mapper) {
    return arr.reduce((acc, curr, i, originalArr) => {
        acc.push(mapper(cur, i, originalArr))
        return acc
    }, [])
}

function filter(arr, callback) {
    return arr.reduce((acc, curr, i, originalArr) => {
        const isAdded = callback(curr, i, originalArr)

        if (isAdded) acc.push(curr)

        return acc

    }, [])

}