# Redux 
The following summary was about this video https://www.youtube.com/watch?v=poQXNp9ItL4 
* Redux is inspire in flux 
* Another popular solution is mux 
* In redux you have to obtain the data from a store to get sincronize the entire application 

Before start you should now about... 
* [Functional programing](#Functional-programing) 
   * [Curring](#Curring) 
* Object oriented 
* Procedual 
* Event driven 
* [Pure functions](#Pure-functions)
* [Inmutability](#Inmutability)
* [About redux](#About-redux)


## Functional programing

In javascript you can use functions as a first class citizens because you can assign a function to a variable. 
also you can send a function as a parameter 

```r
  let input = "     javascript      "; 
  let ouptut = "<div>"+input.trim()+"</div>"
  
  const trim = str => str.trim(); 
  const wranInDiv = str => `<div>${str}</div>`;
  const tolowerCase = str => str.tolowerCase(); 
  
  const result = wranInDiv(tolowerCase(trim(input)));
```
you can create a code as this and is correct, you use little functions to make large solutions. 
but the difficult is you have a lot of parentheses and you have to read them right to left and not left to right as usually 

you can use a library to help you with that, one of those libraries is lodash first you have to run `npm i lodash `

once you have lodash you can use pipe or compose and here is an example 
```r 
  import {compose , pipe } from 'lodash/fp'

  let input = "     javascript      "; 
  let ouptut = "<div>"+input.trim()+"</div>"

  const trim = str => str.trim(); 
  const wranInDiv = str => `<div>${str}</div>`;
  const tolowerCase = str => str.tolowerCase(); 
  //That's exactly the same as before 
  
  const transform = compose(wranInDiv, tolowerCase, trim);  //you send functions as params and compose transforms them into  a new function joining the previous one
  
  const res2 = transform(input); //is the same like const result = wranInDiv(tolowerCase(trim(input)));
```
But you still have the issue you have to read them right to left, to solve that you can use pipe other thing you should know is curring

Because in this kind of functions you can't send two arguments you can use curring and you can transform something like 

```r
  const sum = (a, b) => (a+b)
```
into this 
```r
  const sum2 = a => {
   return function(b){
          return a + b; 
    }
}
```
and that can be reduce into this 
```r
const sum3 = a => b => (a+b) 
```
### Curring

Now we are going to use curring and pipe. 

```r
import { pipe } from 'lodash/fp'

let input = "     javascript      "; 

const trim = str => str.trim(); 
const wrap = type => str => `<${type}>${str}</${type}>`;
const tolowerCase = str => str.toLowerCase(); 

const tr2 = pipe(trim, tolowerCase, wrap("span")); 

const res3 = tr2(input); 

console.log(res3); 
```

## Pure functions
A pure function is a function that whenever you send it the same arguments, the function would return the same return.
in pure functions you can't use things like ramdon or current time or current date or external variables 
* not use random 
* not use current date 
* not use current time 
* not global state (db, dom, files, etc) 

* self-documenting 
* easily testeable 
* concurrency 
* cacheable
In redux reduces are pure functions 

## Inmutability
In redux the store is a Inmutability object. 

|Pros|Cons|
|----|----|
|makes the app more predictible|performance |
|faster change detection |memory overhead|
|concurrency||

* update objects 

```r 
  const person = { name : "John"}; 
  const updated = { ...person, name: "Juan" , age: 24 }; 
```
If your object have more objects inside you have to make deep copy 
```r
  const ob : {elem : 2, arr: { elem : 2 , elem2 : 3}}; 
  const copy = {...ob, arr: {...ob.arr , elem2 : 4 } }; 
  
  //Thats create a copy of ob and change de copy.arr.elem2 to 4
```
* Add to arrays 

If you want to add a new element into an array you should use one of these. 
```r
  const arr = [1,2,3] 
  const arr2 = [0, ...arr] //[0,1,2,3] add in the first position  
  const arr3 = [...arr , 4]//[1,2,3,4] add in the last position 

  //To add in N position 
  const index = arr.indexOf(2)
  const arr4 = [...arr.slice(0, index) , 1.5 , ...arr.slice(index)]
  //arr4 = [1, 1.5, 2, 3] 
  
```
* Updateing an array 
```r
  const number = [1,2,3]; 
  const updated = number.map(n => n === 2 ? 20 : n); 
```

* Removing an array 
```r
  const number = [1,2,3];
  const rem = number.filter(n => n !== 2); //[1,3] 
```
You can also use libraries as Immutable and Immer 

* Example using Immutable 
`npm i immutable `
```r
  /*
    npm i immutable 

    You have to learn about new terms or methods of the library 
    if you want to set a new value they return a new element 
    if you need a json object you have to use .toJS() method 
*/



import {
    Map
} from 'immutable'

let book = Map({
    title: "Javier pomes"
});
function publish(book) {
    return book.set("isPublished", true )
}
book =  publish(book);
console.log(book , book.toJS());
```
* Example using immer 
```r
/*
  npm i immer  
 */

import {
    produce
} from 'immer'

let book = {
    tittle: "Javier pomes"
}

function publish(book) {
    return produce(book, (dBook) => {
        dBook.isPublished = true;
    })
}

let updated  = publish(book);

console.log(book, updated);
```



## About redux
Redux centralized applications state makes data flow transparent and predictable

|Pros|Cons|
|----|----|
|Predictable state changes|Complexity|
|Centralized state |Verbosity |
|Easy debugging ||
|Preserve page state ||
|Undo/redo ||
|Ecosystem of add-ons||


When not to use redux 
* Tight budget 
* small to medium apps 
* Simple UI/data flow 
* Static data 

Redux structure 
* action 
   * represent what happend (EVENT) 
* store 
   * single js object 
* reducer 
   * are the event handler or process 

The reducer are pure functions 
You can log every action 
you can easily implement an undo or do states 

Steps 
* [Install redux ](#Install-redux )
* [Design the store ](#Design-the-store)
* [Define the actions](#Define-the-actions) 
* [Create reducers ](#Create-reducers)
* [Set up the store ](#Set-up-the-store )

In the video him create an easy example about to add, update and remove bugs. 

#### Install redux 
In the course him use the redux 4.0 so i use the same 
`npm i redux@4.0`

* Design the store 
The store is a js file 
and because the store starts out as an empty array I think the easiest way to have a better order is to add a comment about how will the store be.
```r
// I name the file store.js
/*
    [
        {
            id : number, 
            description : "string", 
            resolved : bool 
        }
        , {...}
        , {...} , ...
    ]
*/


import { createStore } from "redux";
import reducer from "./reducer";

const store = createStore(reducer);

export default store; 
```




#### Define the actions 
First you have to think what should your app do and write that. 
* Add bugs 
* Remove bugs 
* Resolve bugs 

Then you can create a js with the names of the actions, in the video him recomends put the names on past so i did it
```r
  // I name the file actionTypes.js 
  export const BUG_ADDED = "bugAdded"; 
  export const BUG_REMOVED = "bugRemoved"; 
  export const BUG_RESOLVED = "bugResolved"
```
If you made an file to have all the actions is easier in this way because if you want to change the name of an action you only have to change it in on place. 
and also you can import like this `import * as actions from "./actionTypes";` and you can get all values with `actions.BUG_RESOLVED` and if you are using vs code they show you the options to autocomplete and that's a good help 

#### Create reducers 

You only have to create one reduce and add a lot of if else or a switch 
you should set state \[] as a default value because when the application start call reduce and become undefined, to avoid that you can use the default value \[] 
```r
//I name this file reducer.js

import * as nActions from "./actionTypes"; 
// I change actions to nActions because with actions you can confuse with action (the parameter of reducer) 

let lastId = 0;

export default function reducer(state = [], action) {
    if (action.type === nActions.BUG_ADDED) {
        return [
            ...state, {
                id: ++lastId,   
                /*
                  I still have a question with this, because you are using a global variable 
                  and i think this is not a pure function
                  but it works and if something work you should not touch it :D 
                */
                description: action.payload.description,
                resolved: false
            }
        ]
    } else if (action.type === nActions.BUG_REMOVED) {
        return state.filter(bug => bug.id !== action.payload.id);
    } else if (action.type === nActions.BUG_RESOLVED) {
        return state.map(bug => bug.id === action.payload.id ? {...bug, resolved : true} : bug);
    }
    //console.log(action.type, "not implemented");
    return state;
}
```

### Create an file for the functions to help yourself 
```r
//I call this file actions.js 
import * as actions from "./actionTypes";

export const bugAdded = description => ({
    type: actions.BUG_ADDED,
    payload: {
        description
    }
});

export const bugSolved = id => ({
    type: actions.BUG_RESOLVED,
    payload: {
        id
    }
})

export const bugDeleted = id => ({
    type: actions.BUG_REMOVED,
    payload: {
        id
    }
})
```


#### Set up the store 
```r
//This is my index.js file 
import store from "./store";
import { bugAdded,bugDeleted,bugSolved } from "./actions";

const unsubscribe = store.subscribe(() => {
    console.log("Store changed!", store.getState());
    //YOU CAN USE THIS TO LOG STATES, OR TO RELOAD ELEMENTS IF YOU ARE USING javascript 
})

store.dispatch(bugAdded("Bug 1"));
//unsubscribe();  //you can run this to quit the suscribe 
store.dispatch(bugSolved(1));

store.dispatch(bugAdded("Bug 2"));

store.dispatch(bugDeleted(2));

```

