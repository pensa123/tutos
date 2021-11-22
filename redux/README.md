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
* About redux[#about-redux]
* Functions as first class citizens

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

you can use a library to help you with that 


### About redux
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

