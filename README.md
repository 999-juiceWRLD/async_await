<h1>Asynchronous JavaScript</h1>

Using too many callback functions could make the code unreadable and pretty confusing. In modern JavaScript, there are tools to deal with asynchronous code. These are called `Promise` and `async/await`.

<h2>Promises</h2>

JavaScript `Promise` is some sort of an assurance that something will be achieved — either done or declined. `Promise` helps manage asynchronous activites.

Mainly, `Promise` has three states:

    - Pending
    - Resolved
    - Rejected

1. **Pending:** This is the ***initial state*** when `Promise` is declared.
2. **Resolved:** This is returned after the operation ***completed successfully***.
3. **Rejected:** If the operation had error during the process, the `Promise` fails.

An example of `Promise`:

```
let promise = new Promise((resolve, reject) => {    
    // Make an asynchronous call and return either resolve or reject
});
```

**NOTE:** You must use a `Promise` method to handle promises.

Another example for `Promise`:

```
let somePromise = new Promise((resolve, reject) => {
    let x = 5;
    if (x === 33) resolve("OK")
    else reject("Wrong")
});

somePromise
    // takes func as param
    .then((val) => { 
        console.log("It worked =>", val);
    })
    .then(() => {
        console.log("Second .then() worked too!");
    })
    // takes func as param
    .catch((err) => {
        console.log("An error occurred — .catch() invoked");
        console.log(".reject() output:", err);
    })
```

Suppose there's two params, `x = 5` and `x = 3`. What matters is that `x` equals to `5`.

```
if x == 5
    It worked => Output: OK
    Second .then() worked too!
else if x == 3
    Output: An error occurred — .catch() invoked
    Output: .reject() output: Wrong
endif
```

<h2>`async/await`</h2>

Instead of always consuming `Promise` with the `.then()` method, we can use `async / await` — in a more comfortable fashion. It’s surprisingly easy to understand and use.

`async` keyword means that this is a special function that is ***asynchronous*** — which keeps running in the background while the rest of the code runs in the **Event Loop**.

This `async` function automatically returns a `Promise`. What's more important is we can always have one or more `await` keyword. However, when an uncaught error is thrown inside the `async` function it wraps the return value in `Promise.catch(returnVal)`.

`await` either returns the value of the fulfilled promise, or throws an error in the case of a rejected promise. We can use regular try-catch to catch the error.

The power of async functions really starts to shine when there are multiple async operations that return promises and which depend on each other.

### `async/await` code examples

1. 

```
const randomProm = new Promise((resolve, reject) => {
    if (Math.random() > 0.5) {
        resolve("Succes");
    } else {
        reject("Failure");
    }
});

// async keyword creates an async function which returns a promise 
async function ansyncExample() {

    try {
        const outcome = await randomProm;
        console.log(outcome);
    } catch (error) {
        console.log(error);
    }

    // This return value is wrapped in a promise
    return 'AsyncReturnVal';
}

// ansyncExample() returns a promise, we can call its corresponding then method
ansyncExample().then((value) => {
    console.log(value);
});

console.log('I get executed before the async code because this is synchronous');

```

2. 

```
// We can use async in function expressions
const randomProm = async () => {
    if (Math.random() > 0.5) {
        // This value is wrapped in Promise.resolve()
        return "Succes";
    } else {
        // This value is wrapped in Promise.reject()
        throw "Failure";
    }
};

// async keyword creates an async function which returns a promise
async function ansyncExample() {
    // randomProm is async fuction which returns a promise which we can await
    return await randomProm();
}

// ansyncExample() returns a promise, we can call its corresponding then/catch method
ansyncExample()
    .then((value) => {
        console.log("Inside then");
        console.log(value);
    })
    .catch((value) => {
        console.log("Inside catch");
        console.log(value);
    });

console.log("I get executed before the async code because this is synchronous");

```
