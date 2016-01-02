# Exploring Async Techniques in JavaScript

## Callbacks

In the JavaScript world, this is the simplest form of asynchronous programming
and is used by almost all of the APIs in the language. It consists of passing
a callback function as an argument in a function call, so the callback function
is called when the desired behavior is supposed to happen.

```js
setTimeout(() => console.log("Hello world!"), 1000);
```

The problem with callbacks is that it can get messy really fast when you are
trying to make more complex programs, which leads to the frequently called
callback hell.

```js
var areThingsComplicated = false,
    timer;

setTimeout(() => {
    console.log("Things can get...");
    setTimeout(() => {
        console.log("complicated.");
        areThingsComplicated = true;

        setTimeout(() => clearTimeout(timer), 1000);
    }, 1000);
}, 2000);

timer = setInterval(() => {
    if(areThingsComplicated) {
        console.log("Oh no!");
    } else {
        console.log("What?");
    }
}, 500);
```

We can improve the situation here if we used named functions to control the flow.

```js
var areThingsComplicated = false,
    timer = setInterval(intervalLoop, 500);

startTimeout();

function startTimeout() {
    setTimeout(firstTimeout, 2000);
}

function firstTimeout() {
    console.log("Things can get...");

    setTimeout(secondTimeout, 1000);
}

function secondTimeout() {
    console.log("complicated.");
    areThingsComplicated = true;

    setTimeout(clearTimerInterval, 1000);
}

function clearTimerInterval() {
    clearTimeout(timer);
}

function intervalLoop() {
    if(areThingsComplicated) {
        console.log("Oh no!");
    } else {
        console.log("What?");
    }
}
```

It's now easier to follow what is happening in the code. The problem with it
is that, each `setTimeout()` call is now coupled with the callback functions.

## Promises

Promises are the solution preferred by the JavaScript community to avoid
callback hell. It defines an API that handles asynchronous events elegantly.
When you have a promise, you can pass a call `.then` passing in a callback
function. But the function returns another promise with the return value of
the previous callback. The advantage is that you can use it to chain callbacks,
making it really simple to compose complicated behaviors.

```js
new Promise(resolve => setTimeout(() => resolve("Hello World!"), 1000))
    .then(value => {
        console.log("Value!");
        return new Promise(resolve => setTimeout(() => resolve("I'll be back"), 1000));
    })
    .then(value => console.log(value));
```

The advantage of chaining callbacks this way is that it maintains a single
indentation level.

```js
var areThingsComplicated = false,
    timer;

function timeout(time, callback) {
    return new Promise(resolve => setTimeout(() => resolve(callback()), time));
}

timeout(2000, () => console.log("Things can get..."))
    .then(() => timeout(1000, () => {
        console.log("complicated.");
        areThingsComplicated = true;
    }))
    .then(() => timeout(1000, () => clearInterval(timer)));

timer = setInterval(() => {
    if(areThingsComplicated) {
        console.log("Not much though.");
    } else {
        console.log("What?");
    }
}, 500);
```

## Coroutines


## Async & Await


## Reactive Extensions


## Communicating Sequential Processes