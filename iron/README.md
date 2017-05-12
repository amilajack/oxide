oxide
=====
Parallelisim and concurrency primatives for JS by leveraging immutability data structures

### Goals
- [ ] API's for threads, thread pool
- [ ] Low overhead FFI
- [ ] Native-feeling API's
- [ ] Async implementations

### IDEA
#### Parallel map/reduce/filter
```js
// parallelArray is immutable
import { ParallelArray } from 'oxide'

// Converting the array to a ParallelArray will make it
// immutable
const array = [1, 2, 3, 4]
const parallelArray = ParallelArray.from(array)

// Map over the array in parallel over 4 threads
parallelArray.map(each => each + 2, 4);

// Run parallel operations asynchronously
parallelArray
    .promise()
    .map(each => each + 2)
    .then(console.log);

// Example with async/await
async function parallelAsyncExample() {
    const results =
        await parallelArray.promise().map(each => each + 2)
    console.log(results)
}
```

#### Thread API
```js
import { Thread } from 'oxide'

const thread1 = new Thread()

thread1.run(() => {
    someExpensiveFn(1000)
})

thread1.run(() => {
    someExpensiveFn(1000)
})
```

### Inspiration
* [node-fibers](https://github.com/laverdet/node-fibers) by @laverdet
