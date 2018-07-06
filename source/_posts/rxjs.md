---
title: RxJS
date: 2018-07-06 11:41:03
tags: #reactive #javascript #rxjs
---
RxJS is used by default in Angular6 to support Reactive Programming. After quick introduction there will be 19 practical code examples using RxJS library.

# INTRODUCTION

## WHAT IS REACTIVE PROGRAMMING?
- Works well with **asynchronous data streams**.
- Data streams can be created from many things (UI Events / HTTP Requests / File Systems / Array-like Objects)- Uses **pipes**.
- Operates on occuring **signals**.

## STREAM
- A sequence of ongoing events ordered in time.
- Emits a value, error and complete signal.

## OBSERVABLES
- **Observables** are used to watch streams and emit functon when a value, error or completed signal is returned.
- **Observables** can be **subscribed to** by an **observer**.
- **Observers** will constantly watch streams and will update accordingly.
- We can interact with data streams as any regular array.

## REACTIVE EXTENSIONS / REACTIVEX / RX
- A library for composing **async** programs by using **observable sequences**.
- Provides a long list of operators which allow us to filter, select, transform, combine and compose observables.
- Examples: **RxJS, Bacon.js, RxPHP, RxJava**.

# CODE EXAMPLES
**Remember to import required methods from RxJS library to work.**

### Example 1 - Basic Observable from an Event
``` javascript
    function initExample1() {
        const button = document.querySelector('button');
        fromEvent(button, 'click')
        .subscribe((event) => console.log(event));
    }
```

### Example 2 - Observable from an Event
``` javascript
    function initExample2() {
        const output = document.querySelector('#output');
        fromEvent(document, 'mousemove')
        .subscribe(
            (event) => {
                output.innerHTML = `Mouse X: ${event.clientX} Mouse Y: ${event.clientY}`;
            }
        );
    }
```

### Example 3 - Observable from an Array
``` javascript
    function initExample3() {
        const numbers = [33, 45, 51, 21, 67];
        const numbersSource = from(numbers);
        const subscribe = numbersSource.subscribe(val => console.log(val));
    }
```

### Example 4 - Observable from an Array of Objects
``` javascript
    function initExample4() {
        const postsElem = document.querySelector('.posts');
        const posts = [
            { id: 1, title: 'Post One', body: 'This is body of the post.'},
            { id: 2, title: 'Post Two', body: 'This is body of the post.'},
            { id: 3, title: 'Post Three', body: 'This is body of the post.'}
        ];
        const postsSource = from(posts);

        const postsSubscribe = postsSource.subscribe(
            post => {
                let liElem = document.createElement("li");
                liElem.innerHTML = post.title;
                postsElem.appendChild(liElem);
            }
        );
    }
```

### Example 5 - Observable from Scratch
``` javascript
function initExample5() {
        const mySource = Observable.create(function(observer) {
            console.log('Creating observable');

            observer.next();

            // observer.error(new Error('Error: Something went wrong'));

            // setTimeout(() => {
            //     observer.next('Next value');
            //     observer.complete();
            // }, 3000);
        });

        const mySubscribe = mySource.subscribe(
            val => console.log(val),
            error => console.error(error),
            complete => console.log('Completed')
        );
    }
```

### Example 6 - Observable from Promise
``` javascript
    function initExample6() {
        const myPromise = new Promise((resolve, reject) => {
            console.log('Creating Promise');
            setTimeout(() => {
                resolve('Hello from Promise');
            }, 3000);
        });

        // myPromise.then(x => {
        //     console.log(x);
        // });

        const mySourceFromPromise = fromPromise(myPromise);
        const subscribeFromPromise = mySourceFromPromise.subscribe(val => console.log(val));
    }
```

### Example 7 - AJAX request to GitHub API to get user
``` javascript
    function initExample7() {
        const usernameElement = document.querySelector('#username');
        const nameElement = document.querySelector('#name');
        const bioElement = document.querySelector('#bio');
        const inputElement = document.querySelector('#myInput');

        function getUser(username) {
            return fetch(`https://api.github.com/users/${username}`)
                .then(response => response.json());
        }

        fromEvent(inputElement, 'keyup')
        .debounceTime(350)
        .subscribe((event) => {
            fromPromise(getUser(event.target.value))
            .subscribe(user => {
                console.log(user);
                usernameElement.innerHTML = user.login;
                nameElement.innerHTML = user.name;
                bioElement.innerHTML = user.bio;
            });
        });
    }
```

### Example 8 - Interval
``` javascript
    function initExample8() {
        const source = interval(500)
        .take(5)
        .subscribe(
            val => console.log(val),
            err => console.error(err),
            complete => console.log('Completed')
        );
    }
```

### Example 9 - Timer
``` javascript
    function initExample9() {
        const source = timer(3000, 500)
        .take(5)
        .subscribe(
            val => console.log(val),
            err => console.error(err),
            complete => console.log('Completed')
        );
    }
```

### Example 10 - Range
``` javascript
    function initExample10() {
        const source = range(100, 10)
        .subscribe(
            val => console.log(val),
            err => console.error(err),
            complete => console.log('Completed')
        );
    }
```

### Example 11 - Map
``` javascript
    function initExample11() {
        const source = interval(1000)
        .take(10)
        .map(value => value * 2)
        .subscribe(value => console.log(value))
    }
```

### Example 12 - Map Chaining
``` javascript
    function initExample12() {
        const source = from(['Adam', 'Peter', 'Damian'])
        .map(v => v.toUpperCase())
        .map(v => 'My name is ' + v)
        .subscribe(value => console.log(value))
    }
```

### Example 13 - Map Fetched data
``` javascript
    function initExample13() {
        function getUser(username) {
            return fetch(`https://api.github.com/users/${username}`)
                .then(response => response.json());
        }

        fromPromise(getUser('khamian'))
        .map(user => user.bio)
        .subscribe(bio => {
            console.log('Bio: ' + bio);
        });
    }
```

### Example 14 - Pluck
``` javascript
    function initExample14() {
        const users = [
            { name: 'Damian', age: 25 },
            { name: 'Peter', age: 28 },
            { name: 'Adam', age: 31 },
        ]

        const stream = from(users)
        .pluck('name')
        .subscribe(x => console.log(x))
    }
```

### Example 15 - Merge
``` javascript
    function initExample15() {
        of('Hello')
        .merge(of('Everyone'))
        .subscribe(x => console.log(x))
    }
```

### Example 16 - Merge + Interval
``` javascript
    function initExample16() {
        // interval(2000)
        // .merge(interval(500))
        // .take(25)
        // .subscribe(x => console.log(x));

        // Better syntax Merge:
        const source1 = interval(2000)
        .map(v => 'Source1: ' + v);

        const source2 = interval(500)
        .map(v => 'Source2: ' + v);

        merge(source1, source2)
        .take(25)
        .subscribe(x => console.log(x));
    }
```

### Example 17 - Concat
``` javascript
    function initExample17() {
        const source1 = range(0, 5)
        .map(v => 'Source1: ' + v);

        const source2 = range(6, 5)
        .map(v => 'Source2: ' + v);

        concat(source1, source2)
        .subscribe(x => console.log(x));
    }
```

### Example 18 - mergeMap
```javascript
    function initExample18() {
        // Wrong Way
        // of('Hello')
        // .subscribe(v => {
        //     of(v + ' Everyone')
        //     .subscribe(x => console.log(x))
        // });

        // Good Way - Using mergeMap
        of('Hello')
        .mergeMap(v => {
            return of(v + ' Everyone');
        })
        .subscribe(x => console.log(x));
    }
```

### Example 19 - switchMap (Improved GitHub Fetch Example)
``` javascript
    function initExample19() {
        const usernameElement = document.querySelector('#username');
        const nameElement = document.querySelector('#name');
        const bioElement = document.querySelector('#bio');
        const inputElement = document.querySelector('#myInput');

        function getUser(username) {
            return fetch(`https://api.github.com/users/${username}`)
                .then(response => response.json());
        }

        fromEvent(inputElement, 'keyup')
        .debounceTime(350)
        .map(event => event.target.value)
        .switchMap(v => {
            return fromPromise(getUser(v));
        })
        .subscribe(user => {
            console.log(user);
            usernameElement.innerHTML = user.login;
            nameElement.innerHTML = user.name;
            bioElement.innerHTML = user.bio;
        })
    }
```

## SOURCES
    https://github.com/khamian/rxjs-sandbox
    https://rxjs-dev.firebaseapp.com/
    https://www.learnrxjs.io/
    https://www.youtube.com/watch?v=ei7FsoXKPl0