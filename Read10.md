# The Call Stack

A call stack is a mechanism for an interpreter (like the JavaScript interpreter in a web browser) to keep track of its place in a script that calls multiple functions — what function is currently being run and what functions are called from within that function, etc.

When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the functionو Any functions that are called by that function are added to the call stack further up and run where their calls are reached, When the current function is finished, the interpreter takes it off the stack and resumes execution where it left off in the last code listing, If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.

Ex:
```
function greeting() {
   // [1] Some codes here
   sayHi();
   // [2] Some codes here
}
function sayHi() {
   return "Hi!";
}

// Invoke the `greeting` function
greeting();

// [3] Some codes here
```

it will call the greeting function t first then the sayHi function and return "Hi !"
\*********************************************************************************************************************************************************************************\*

