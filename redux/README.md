# Redux 
The following summary was about this video https://www.youtube.com/watch?v=poQXNp9ItL4 
* Redux is inspire in flux 
* Another popular solution is mux 
* In redux you have to obtain the data from a store to get sincronize the entire application 

Before start you should now about... 
* Functional programing 
* Object oriented 
* Procedual 
* Event driven 
* [About redux](#About-redux)


### Functional programing

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

