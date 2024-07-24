# The main purposse of this is to get the NODE JS certification

The content of this guide was retreade from this [course](https://trainingportal.linuxfoundation.org/learn/course/nodejs-application-development-lfw211) to get the NODE JS certification 

## How to start 

The best way to install and use nodejs, is with a version control like NVM 
once you have nvm installed, in this course they are usign npm 20.*.* so you have to run `nvm install 20` and then use `nvm use 20` if you are not using other version of node you can set it as default 

## NodeJS commands 

### node -i v 

it gets the 

### node --help 

This is used to get help in nodejs 

### node --v8-options

This get specifics options for the version of node

### node --stack-trace-limit

This could be used to increese the trace limit when an error occur it have to be used like 
  `node --stack-trace-limit=99999 app.js`

### node --print o node -p

This execute an action and print the result 
Exmaple: 
  `node -p "2+2"` it returns an 4 
  `node -p "console.log(2+2)" ` it return undefined and print a 4 

### node --eval or node -e

This only execute an action 
Examples:
  `node -e "4+4"` it do nothing
  `node -e "console.log(2+2)" ` it only print a 4

### node -r file.js

It pre-charge a file 

Given two files preload.js and app.js

Preload.js

		console.log('preload.js: this is preloaded')

And a file called app.js containing the following:

		console.log('app.js: this is the main file')

The following command would print preload.js: this is preloaded followed by app.js: this is the main file:

`node -r ./preload.js app.js`

			preload.js: this is preloaded
			app.js: this is the main file

## Debugging tools

For debugging tools there are two commands, node --inspect and also no --inspect-bkr 
with both it create an url to watch that but the easier way is open google-chrome browser and search  `chrome://inspect/#devices` [This url](chrome://inspect/#devices)

and with bouth we can use debugger on the js file to debug that specific part, without adding the breakpoint on the browser 

### node --inspect app.js

It helps to inspect the code and watch the process but doesn't allow breakpoints

### node --inspect-bkr app.js

It helps to inspect the code and watch the process but it allows breakpoints
