# Introduction to EJS

### Getting Started With EJS:

- in your terminal, type ``` git init -y ``` then ``` npm --save express body-parser cors ejs ``` 
- in the editor, exactly on server.js file type 
```
var express = require('express');
var bodyParser = require('body-parser');
var cors = require('cors');
var path = require('path');

var app = express();

app.use(bodyParser());
app.use(cors());

app.set('views',path.join(__dirname,'views'));
app.set('view engine','ejs');

app.get('/',function(request,response){
  response.render('index');
});

app.listen(3000,function(){
  console.log('listening to the port');
}
```
- make a new file called: views/index.ejs and right your html code inside it.


### How To Inject Value Into Our EJS:
- inside server.js file:
```
app.get('/',function(request,response){
  response.render('index',{
    foo: 'bar'
  });
});
```
- inside index.ejs file:
```
<h1>hello<%= foo %></h1>
```

### For Loops & Arrays
- inside server.js file:
```
app.get('/',function(request,response){
  response.render('index',{
    foo: 'bar'
  });
});
```
