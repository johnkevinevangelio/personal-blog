---
layout: post
title:  "Callback, Promise and Async/Await in Javascript"
date:   2020-09-1 02:07:20 +0800
---

### What is Javascript?

JavaScript is a **single threaded**, event driven language. What that means is unlike traditional languages like Java/C#, JavaScript cannot spawn multiple threads to do some processing in parallel.

**Meaning it can handle one task at a time or a piece of code at a time.**

### Synchronous vs Asynchronous Programming
**Synchronous (blocking)** - execute one thing at a time. Needs to wait for the other process to finish before moving to the next one.

**Asynchronous (non-blocking)** - execute multiple things at a time and you don't have to finish executing the current thing in order to move on to the next one. 


### How does Callback works in Javascript?

![Callback process](/personal-blog/assets/blog2/callback.jpg)
##### Source: *<https://scotch-res.cloudinary.com/image/upload/w_1050,q_auto:good,f_auto/media/4974/xCkAPcmuQNqQCGpO2avR_Event-loop.png.jpg>*

1. **Call Stack** : It’s a data structure which records the function calls, basically where in the program we are. If we call a function to execute , we push something on to the stack, and when we return from a function, we pop off the top of the stack.

For Chrome browser, there is a limit on the size of the stack which is 16,000 frames , more than that it will just kill things for you and throw Max Stack Error Reached.

2. **Heap** : Objects are allocated in a heap i.e mostly unstructured region of memory. All the memory allocation to variables and objects happens here.

3. **Queue** : A JavaScript runtime contains a message queue, which is a list of messages to be processed and the associated callback functions to execute.

4. **Event loop** : basic job is to look both at the stack and the task queue, pushing the first thing on the queue to the stack when it see stack as empty.

**What is Callback in Javascript?**
A JavaScript Callback Function is a function that is passed as a parameter to another JavaScript function, and the callback function is run inside of the function it was passed into.

**Why we use callback?**

Sample Code:

Function without callback:

    let calc = (num1, num2, calcType) => {
        if (calcType === 'add') {
            return num1 + num2
        } else if ( calcType === 'multiply') {
            return num1 * num2
        }
    }
    console.log(calc(2, 3, 'add'))

With Callback:

    let add = (num1, num2) => {
        return num1 + num2
    }
    let multiply = (num1, num2) => {
        return num1 * num2
    }
    let calc = (num1, num2, callback) => {
        return callback(num1, num2)
    }
    
    console.log(calc(2, 3, add))
    console.log(calc(2,3, (num1, num2) => {
        return num2 - num1
    }))

### Promise

Just like a PROMISE in real life, what you do is that promise on something by saying for example, I promise to make you happy. That promise has two results, either  that promise is resolved or that promise is failed or rejected.

#### Actual Syntax of Promise

    let p = new Promise((resolve, reject) => {
        let a = 1 + 1
        if (a == 2) {
            resolve('Success')
        } else { 
            reject('Failed')
        }
    })

    p.then((message) => {
        console.log('This is in the then' + message)
    }).catch((message) => {
        console.log('This is in the catch' + message)
    })

Promises are really great when you need to do something that's going to take a long time in the background such as downloading an image from a different server and you just want to do something after it's complete instead of making everything else wait for it and then also you can catch it to see if it's failed. In that way you can maybe retry it or give the user an error message if it fails.

#### Using promises instead of callbacks

**Sample Callback function**

    const userIsSick = false
    const userHasWork = false
    function watchMovieCallback(callback, errorCallback) {
        if (userIsSick) {
            errorCallback({
                name: 'User is Sick',
                message: ':('
            })
        } else if (userHasWork) {
            errorCallback({
                name: 'User has Work',
                message: 'Work > you'
            })
        } else {
            callback('Success!')
        }
    }
    watchMovieCallback((message) => {
        console.log(message)
    }, (error) => {
        console.log(error.name + ' ' + error.message)
    })

**Convert to Promise**
    
    function watchMoviePromise () {
        return new Promise((resolve, reject) => {
            if (userIsSick) {
                reject({
                    name: 'User is Sick',
                    message: ':('
                })
            } else if (userHasWork) {
                reject({
                    name: 'User has Work',
                    message: 'Work > you'
                })
            } else {
                resolve('Success!')
            }
        })
    }
    watchMoviePromise().then((message) => {
        console.log(message)
    }).catch((error) => {
        console.log(error.name + '\n' + error.message)
    })
    
Other Example Code:

    const downloadImageOne = new Promise((resolve, reject) => {
        resolve('Image 1 Downloaded')
    })
    const downloadImageTwo = new Promise((resolve, reject) => {
        resolve('Image 2 Downloaded')
    })
    const downloadImageThree = new Promise((resolve, reject) => {
        resolve('Image 3 Downloaded')
    })
    Promise.all({
        downloadImageOne,
        downloadImageTwo,
        downloadImageThree
    }).then((message) => {
        console.log(messages)
    })
    Promise.race([
        downloadImageOne,
        downloadImageTwo,
        downloadImageThree        
    ]).then((message) => {
        console.log(messages)
    })

### Async/Await


**Async** - The word “async” before a function means one simple thing: a function always returns a promise. Other values are wrapped in a resolved promise automatically.

**Await** - The keyword await makes JavaScript wait until that promise settles and returns its result.

Sample Codes:
    
    function makeRequest(location) {
        return new Promise((resolve, reject) => {
            console.log('Making Request to ${location}')
            if (location === 'Google') {
                resolve('Google says hi')
            } else {
                reject('We can only talk to Google')
            }
        })
    }

    function processRequest(response) {
        return new Promise((resolve, reject) => {
            console.log('Processing response')
            resolve('Extra Information + ${response}')
        })
    }

Using Promise:

    makeRequest('Google').then(response => {
        console.log('Response Received')
        return processRequest(response)
    }).then(processedResponse => {
        console.log(processedResponse)
    }).catch(err => {
        console.log(err)
    })

Using Async/Await:

    async function doWork() {
        try {
            const response = await makeRequest('Google')
            console.log('Response Received')
            const processResponse = await processRequest(response)
            console.log(processResponse)
        } catch (err) {
            console.log(err)
        }
    }
    doWork()


### Sources:

1. *[Web Dev Simplified](https://www.youtube.com/channel/UCFbNIlppjAuEX4znoulh0Cw)*
2.  *[Scotch](https://scotch.io/@pratikpandey/how-does-callback-work-in-javascript)*
3.  *[JAVASCRIPT.INFO](https://javascript.info/async-await)*
4.  *[codeSTACKr](https://www.youtube.com/channel/UCDCHcqyeQgJ-jVSd6VJkbCw)*
5.  *[techsith](https://www.youtube.com/channel/UCDCHcqyeQgJ-jVSd6VJkbCw)*