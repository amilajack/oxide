oxide
=====

[![Greenkeeper badge](https://badges.greenkeeper.io/amilajack/oxide.svg)](https://greenkeeper.io/)

Parallelisim and concurrency primatives for JS by leveraging immutability data structures

⚠️**WORK IN PROGRESS. DOES NOT WORK AT THE MOMENT**⚠️

### Goals
- [ ] API's for threads, thread pool
- [ ] Low overhead FFI
- [ ] map/filter/reduce implementation w/ work stealing
- [ ] Promise-based API (by default), provide sync implementations as well
- [ ] Immtuable Data Structures (bitmapped vector trie, hash array mapped trie, etc), reduce copying for better perf

### IDEA
#### Parallel map/reduce/filter
```js
// parallelArray is immutable
import { ParallelArray } from 'oxide'

// Converting the array to a ParallelArray will make it
// immutable
const array = [1, 2, 3, 4]
const parallelArray = ParallelArray.from(array)

// Map over the array in parallel over 4 threads asynchronously
// .map(), .filter(), .reduce() implementations are all executed in parallel
parallelArray
    .map(each => each + 2, 4)
    .then(console.log)

// Example with async/await
async function parallelAsyncExample() {
    const results = await parallelArray.map(each => each + 2)
    console.log(results)
}
```

#### Promise-Based API
Ideally, oxide would be async by default, which align's well with node's convention:
```js
import { Thread } from 'oxide'

const thread1 = new Thread()
const thread2 = new Thread()

Promise.all([
    thread1.run(() => someExpensiveFn(1000)),
    thread2.run(() => fib(1000)),
    thread2.run(() => {
        for (let i = 0; i < 10000; i++) {
            console.log('moo')
        }
    })
])

```

You can also call API's synchronously:

```js
thread1.runSync(() => {
    someExpensiveFn(1000)
})

const result: number = thread2.runSync(() => fib(1000))
```

### Inspiration
* [node-fibers](https://github.com/laverdet/node-fibers)
* [node-threads-a-gogo](https://github.com/xk/node-threads-a-gogo)
* [RiverTrail](https://github.com/IntelLabs/RiverTrail)
