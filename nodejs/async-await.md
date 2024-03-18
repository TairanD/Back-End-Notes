# async & await

## 1 - async
We can use keyword `async` to create an asynchronous function. The return value of this asynchronous function will be 
encapsulated into a Promise.

For example, the following function will actually return a Promise. We need to use `fn().then(r => console.log(r)` to get the value.
```javascript
const fn = async () => {
    return 10
}
// it is equal to the following code, but in a more concise way
const fn = () => {
    return Promise.resolve(10)
}
```

## 2 - await
In the asynchronous functions declared by `async`, we can use keyword `await` to execute asynchronous functions.

- Promise chain can resolve 'recall hell'. Yet, when one chain is too long, codes are still seem redundant and not elegant.
Therefore, we would like to execute asynchronous code in a synchronous way.

Thus, when we use await to execute the synchronous code, it will pause the running of the program and wait the result of 
the synchronous code. The synchronous function will rerun only when the result is generated.

## 3 - Big Confusion
However! The purpose of synchronous code is to prevent congestion of synchronous code. Therefore, does `await` contradict with
this original objective?

In the definition, we should notice that `await` can only be used inside the asynchronous functions declared by `async`. 
This design can prevent `await` from congestion.
- a.k.a. **_awaiting code inside an `async` will only pause the code inside the
`async` function, and do not influence the outside code like a normal asynchronous function do._**

At this time, some will argue `await` still impact the internal node. Yet, usually, code after `await` relies on the result
of `await` function.

## 4 - Error Process
When we use async-await, we find we cannot use `finally` to deal with errors. Therefore, we need to use try-catch to do it.

```javascript
const fetchData = async () => {
    try{
        const response = await fetch('/')
        // further process response
    }catch (error){
        console.error("Error:", error)
    }
}
```

## *Note
- What happen if the code inside an async declared function is synchronous?
   - The code will be executed synchronously
- When we add `await` before a piece of synchronous code:
  - the code behead the `await` code inside the current `async` function will be added to the micro task stack
  - For example, the following code will output 1243
    ```javascript
    async function fn(){
        console.log(1)
        await console.log(2)
        // all code below will be added to the micro task stack
        console.log(3)
    }
    
    fn()
    console.log(4)
    ```
    and it is equal to the following code, the output is still 1243
    ```javascript
    async function fn(){
        return new Promise(function (resolve) { 
            console.log(1)
            console.log(2)
            resolve()
        }).then(r => console.log(3))
    }
    
    fn()
    console.log(4)
    ```