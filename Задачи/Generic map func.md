function map<T,R>(arr:T[], func:(v: T, i: number,arr: T[]) => R): R[] {
    return arr.map(func)
}