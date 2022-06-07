## Hola!

The best time to learn Markdown is 10 years ago, the second is __now__!

EME is trying to give you the best experience of writing in Markdown.

Markdown shines ✨

![shiny](http://ww4.sinaimg.cn/large/a15b4afegw1f6499jo5ruj20iw0anmxv.jpg)

## Focus Mode

The first feature that most markdown editors do not have is, focus the paragraph you are actually writing. It reduces a lot of distractions when you are working on a long post. Use <kbd>Ctrl/CMD + \</kbd> to toggle it.




## Javascript Objects

```js  
var Student = function(name, age) {
  this.name = name; 
  this.age = age;
  this.birthday = function(){
   this.age++;
  }
}

var student1 = new Student("Mary", 11);
student1.birthday();


console.log("Student 1 Age:", student1.age);
```
### Constructing New Object

```js
var student5 = new Student("Ayush", 23);
```

### Adding New Properties To Objects

```js
Student.prototype.grade = "A";
```
This adds grade property to object


## Object Instance Methods

![](/home/rane/ashu/Markdowms/javascript/object_instance_method.png)

## var VS let VS const

const once defined cannot be redefined
```js
const constant = 'I am a constant';
constant = " I can't be reassigned";

// Uncaught TypeError: Assignment to constant variable
```

The first opinion comes from Mathias Bynes:

* Use const by default
* Use let only if rebinding is needed.
* var should never be used in ES6.

The second opinion comes from Kyle Simpson:

* Use var for top-level variables that are shared across many (especially larger) scopes.
* Use let for localized variables in smaller scopes.
* Refactor let to const only after some code has to be written, and you’re reasonably sure that you’ve got a case where there shouldn’t be variable reassignment.

## Arrow Functions

```js
var greeting = (name) => {
  console.log(`hello ${name}`);
}
```
