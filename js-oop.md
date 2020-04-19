# JS OOP & MVC

- [x] V: "Object-Oriented Programming with JS", "https://www.packtpub.com/application-development/object-oriented-programming-javascript-build-quiz-app-video"
- [x] V: "Develop a Quiz App with Javascript-OOP", "https://www.youtube.com/watch?v=jvk1pFNqXaw"
- [x] "Object-oriented JS for beginners", "https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS" },
- [x] "An introduction to Object-Oriented Programming in JS", "https://www.freecodecamp.org/news/an-introduction-to-object-oriented-programming-in-javascript-8900124e316a/"
- [x] "OOP In JS: What You NEED to Know", "https://javascriptissexy.com/oop-in-javascript-what-you-need-to-know/"
- [x] "JS: The Right Way", "http://jstherightway.org/"
- [x] "Object-Oriented Programming With JS", "https://code.tutsplus.com/articles/Object-Oriented-Programming-JavaScript--cms-29256"
- [x] "Wiki: MVC" , "https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller"
- [ ] "Wiki: OOP" , "https://en.wikipedia.org/wiki/Object-oriented_programming"
- [ ] "The MVC Design Pattern in Vanilla JS" , "https://www.sitepoint.com/mvc-design-pattern-javascript/"
- [ ] "JS MVC – A List Apart" , "https://alistapart.com/article/javascript-mvc/"
- [ ] "A Walk-through of a Simple JS MVC Implementation" , "https://medium.com/@ToddZebert/a-walk-through-of-a-simple-javascript-mvc-implementation-c188a69138dc"
- [ ] "Model-View-Controller (MVC) in JS" , "https://alexatnet.com/model-view-controller-mvc-in-javascript/"
- [ ] "MVC for JS Developers" , "https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch10s02.html"
- [ ] "Writing a Simple MVC App in Plain JS – Tania Rascia", "https://www.taniarascia.com/javascript-mvc-todo-app/"
- [ ] V: "How to Build a JS Application Project", "https://www.youtube.com/watch?v=qYNbaIJKBNE"

## MVC: Concepts

- a software design pattern used for developing user interfaces
- divides the related program logic into three interconnected elements
- Model: The central component of the pattern. It is the application's dynamic data structure, independent of the user interface. It directly manages the data, logic and rules of the application
- View: Any representation of information such as a chart, diagram or table. Multiple views of the same information are possible, such as a bar chart for management and a tabular view for accountants.
- Controller: Accepts input and converts it to commands for the model or view.
- Workflow:
  - The model is responsible for managing the data of the application. It receives user input from the controller.
  - The view means presentation of the model in a particular format.
  - The controller responds to the user input and performs interactions on the data model objects.
  - The controller receives the input, optionally validates it and then passes the input to the model.

## MVC: Examples

## OOP: Concepts

- OOP is very abstract
- an object is based on a real world example, e.g. a person or a product
- an object can contain data and perform some logic based on its data
- classes are the templates used to instantiate objects
- an application is a collection of objects that communicate with each other
- as a result, OOP code is very easy to understand
- you have to decide how to break an application into these small objects in the first place
- Object: the main actors in an application, the building blocks that do all the work.
- Encapsulation: refers to enclosing all the functionalities of an object within that object so that the object’s internal workings are hidden from the rest of the application. This allows us to abstract or localize specific set of functionalities on objects. The only way to access the data is indirect via the functions written into the objects.
- Inheritance: refers to an object being able to inherit methods and properties from a parent object. The new object “inherits” all of the features of its parent, avoiding the creation of new code from scratch. Furthermore, any changes made to the parent class will automatically be available to the child class.

## OOP: Examples

Function:

```js
// normal function, used without `new`
// object has to be setup and returned manually
function PersonNoNew(name) {
  const obj = {};
  obj.name = name;
  obj.greeting = () => {
    console.log(`Hi, ${obj.name}`);
  };
  return obj;
}

const bobNoNew = PersonNoNew('BobNoNew');
bobNoNew.greeting();
```

Function with `new`

```js
// normal function, used with `new`
// no need to declare object, implicit return
function PersonWithNew(name) {
  this.name = name;
  this.greeting = () => {
    console.log(`Hi, ${this.name}`);
  };
}

const bobWithNew = new PersonWithNew('BobWithNew');
bobWithNew.greeting();
```

Class:

```js
// class syntax, used with `new`
// constructor instead of function parameter, implicit return
class PersonClass {
  constructor(name) {
    this.name = name;
  }
  greeting() {
    console.log(`Hi, ${this.name}`);
  }
}

const bobClass = new PersonClass('BobClass');
bobClass.greeting();
```
