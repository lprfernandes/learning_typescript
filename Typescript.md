# Typescript
Is a superset of Javascript, which adds data types, static typing, classes, interfaces,etc.

## First app in ts
1. Install visual studio code
2. Install nodejs
3. Install typescript by going to a cmd run the command npm install typescript -g
4. To compile the code run tsc nameOfTheFile.ts

# Defining vars
We can define vars by 2 keywords var and let. They both can create global vars if used outside a method and local if used inside.
The difference is the var keyword can't create the block level var.
The second difference is that the vars created with the "let" keyword aren't hoisted to the top of it's scope, meaning are not accessible if called before declared. (var is hoisted)

    var message : string = "Hello World"
    console.log(message);

# Data Types

- number => 1;1.3;-10
- string => 'a';"a";`a`;
- boolean => true; false; => no truthy or falsy values
- any => {firstName:"Adam", age: 30 }

    //defining the var types of an object
    const person: {
      name: string;
      age: number;   
    } = {
      name: "Luis",
      age: 36
    }


- array => [1,2,3]

  let favoriteActivities: string[];
  favoriteActivities = ["Sports","Reading"];

  for (const hobby of favoriteActivities){
  console.log(hobby.toUpperCase());
  }


- tuple => [2; "author"]
  const person: {
    name: string,
    age: number
    ...
    role: [2, "author"]
  }


- enum => Role {ADMIN, READ_ONLY, AUTHOR};
          Role {ADMIN = 5, READ_ONLY = 100, AUTHOR = 200}

  ...if(Role.ADMIN){
    ...
  }

- any => let favoriteActivities: any[];
//we want to avoid it whenever possible because we return basically to vanilla javascript


## Union Types
//var or arg accepts ex number or string

function combine(input1: number | string, input2: number | string){
  
  if(typeof input1 === "number" && typeof input2 === "number"){
    return input1 + input2;
  }
  else {
    return input1.toString() + input2.toString();
  }
}

## Literal Types
//takes a value as a type (like a constant)

resultConversion = "as-number" | "as-text"
const number2 = 2.8


## Type aliases

    type myCustomType = number | string;

    function combine(input1: myCustomType, input2: myCustomType){
      ....
    }

    type User = { name: string; age: number };
    const u1: User = { name: 'Max', age: 30 }; 

# Functions
There can be 2 types of functions, with function syntax or with arrow function syntax.

      getResult: function() {
        //if we use "this" keyword in function syntax it refers to the present object
      }

      getResult: () => {
        //if we use "this" keyword in arrow syntax it refers to the parent object in which the function is created.
      }

## Return types
    
    // explicit the return type of the function
    function add(n1: number, n2: number) : number {
      ...
    }

    //or void
    function printResult(num:number) : void {
      console.log("result is:" + num);
    }

## var as functions
create a var of type function (will be pointing at a function in the future)

    let combineValues: Function;

    let combineValues: () => number; //combineValues will be pointing to a function that takes in no args and outputs a number
    let combineValues: (a: number, b: number) => number; //combineValues will be pointing to a function that takes in 2 number args and outputs a number

## callbacks

//defining the function
function addAndHandle(n1:number, n2: number, cb: (n1:number) => void){
  const result = n1 + n2;
  cb(result);
}

//calling the function
addAndHandle(10, 20, (result)=> {console.log(result)});

## Unknown Type

let userInput: unknown;

similar to any but allows to cast from unknow to other type vars if we check its type with an if condition.


## Never function return
Because this function will never return anything

function generateError(message:string, code:number) : never {
  throw {message: message, errorCode: code};
}

generateError("An error occurred!", 500);


# Compiler
//watch mode - recompiles on change 
tsc app.ts -w

//compiling entire project, on the project dir
tsc --init
//it creates the tsconfig.json file, then run only 
tsc //or combine it with the watch -w (tsc -w) to watch the entire project all the time
// it will compile every typescript file into javascript

## Include and Exclude Options
In the tsconfig.json after all the options we can add exclude to exclude files from compiling:

  , "exclude" : [
    "node_modules" //by default they're excluded already
    "analytics.js",
    "someOtherFile.js",
    "*.dev.ts", //any file with tthis extension
    "**/*.dev.ts", // any file in any folder with this extension
  ],
    "include": [ //if we add the "include" key, then we have to specify every file we want to compile
      "app.ts",
      ...
    ]

  "target" => js version
  "lib" => if commented in, we no longer work with the default pre-packed libs but we must specify them here.
  "sourceMaps" => allows the browser debugger to see ts instead of js
  "outDir" => used to send the output of the compilation to the dist folder while our ts files are in the src folder
  "rootDir" =>  used to select the files that will be compiled. (differs from include in the way it also follows/creates subfolders structures)


# Classes

  //To define a class
   class Department {
      private name: string;
      protected employees: string[] = [];

      //ctor
      constructor(id:string, n: string){
        this.name = n;
      }

      //create a method
      describe(this: Department){
        console.log("Department: " + this.name);
      }

      addEmployee(employee: string){
        this.employees.push(employee);
      }

      printEmployeeInfo(){
        console.log(this.employees.length);
        console.log(this.employees);
      }
   }

  //To create a new instance of the class
  const accounting = new Department("Accounting");
  console.log(accounting);

  accounting.describe();
  accounting.addEmployee("Luís");
  accounting.addEmployee("Manel");
  accounting.printEmployeeInfo();


//Shorthand initialization
Instead of having the ctor initializing the private fields like so:

  class Department {

      private name: string;

      constructor(n: string){
        this.name = n;
      }
  }

  //we can directly initialize it on the ctor args like so:

  class Department{

      constructor(private readonly n: string){
        
      }
  }

## Inheritance
//keyword super calls the base class ctor

  class ITDepartment extends Department {
    constructor(id: string) {
      super(id: string, "IT");
      ...
    }

    //override base class addEmployee
    addEmployee(employee: string){
      this.employees.push(employee);
    }
  }

  const myDepartment = new ITDepartment("d2");

## Getters and Setters

  private lastReport: string;

  get mostRecentReport() {
    return this.lastReport;
  }

  set mostRecentReport(value: string) {
    this.lastReport = value;
  }

  ...and to use it outside the class

  //Getting: we use it without the "()"
  console.log(accounting.mostRecentReport);

  //Setting
  accounting.mostRecentReport = "cenas";

## Static methods and props
Hability to create methods and props that are called directly from the class without having to create an instance of it.


  static createEmployee(name : string) {
    return { name : name};
  }

  ...

  Department.createEmployee("Luís);

//and the prop

  static fiscalYear = 2023;

  ...

  console.log(Department.fiscalYear);


## Abstract class

  abstract describe(this: Department): void;


## Singletons & Private Ctors
If we want to create a singleton, meaning a single instance of a class and not allow more, we can create a class with a private ctor and then create the instance there. 
Like so:

  private static instance: AccountingDepartment;

  private contructor(id: string, private reports: string[]){
    ...
  }

  static getInstance(){
    if (this.instance) {
      return this.instance;
    }
    this.instance = new AccountingDepartment();
    return this.instance;
  }

# Interfaces

  interface Person {
    name: string;
    age: number;

    greet(phrase: string) : void;
  }

let user1: Person;
...

user1 = {
  name: "Max",
  age: 30,
  greet(phrase: string){
    console.log(phrase + " " + this.name);
  }
};

class Person implements Greetable

## Readonly interface prop
Says that the prop is only setable once in its initialization.

  readonly name: string

## Interface as an Anonymous function

  interface AddFn {
    (a: number, b: number) : number;
  }

let add: AddFn;

add = (n1: number, n2: number) => {
  return n1 + n2;
}

## Optional Params & Props
//this tells typescript the prop can exist or not in the classes that implement this interface

  interface Named {
    outputName?: string; 
  }

# Advanced Types

## Intersection Types

  type Admin = {
    name : string,
    privileges: string [];
  };

  type Employee = {
    name : string,
    startDate: Date;
  };

  type ElevatedEmployee = Admin & Employee;

  const e1 : ElevatedEmployee = {
    name : "Max",
    privileges: ["Create-server"],
    startDate: new Date()
  }

## Type Guards


//method with "name of the prop" in var

type UnknownEmployee = employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee){
  console.log("Name: " + emp.name);
  if("privileges" in emp){
    console.log("Privileges: " + emp.privileges);
  }
  if("startDate" in emp){
    console.log("Start date: " + emp.startDate);
  }
}

//method with the instance of

function printEmployeeInformation(emp: UnknownEmployee){
  console.log("Name: " + emp.name);
  if(emp instance of Admin){
    console.log("Privileges: " + emp.privileges);
  }
}

## Discriminated Unions
Make one common prop between classes that describes the object


  interface Bird {
    type: "bird";
    flyingSpeed: number;
  }

  interface Horse {
    type: "horse";
    runningSpeed: number;
  }
  
  type Animal = Bird | Horse;

  function moveAnimal(animal: Animal) {
    let speed;
    switch (animal.type)
      case "bird":
        speed = animal.flyingSpeed;
        break;
      case "horse":
        speed = animal.runningSpeed;
        break;
    
    console.log(speed);
  }

  moveAnimal({type: "bird", flyingSpeed: 10});


## Type Casting

const userInputElement = <HTMLInputElement>document.getElementById("user-input");
//or
const userInputElement = document.getElementById("user-input") as HTMLInputElement;

## Index Properties
When we don't know in advance the prop name but we want to condition its type

// I want to be able to pass {email: "Not a valid email", username: "something something"}
  interface ErrorContainer { 
    [prop: string] : string;
  }

  const errorBag: ErrorContainer = {
    email: "Not a valid email"
    username: "Something something error"
  }


## Function overloads
Can be used to desambiguate the return types of union or combined types

  type Combinable = string | number;

  function add(a: number, b: number);
  function add(a: string, b: string);
  function add(a: Combinable, b: Combinable) {
    return a + b;
  }

## Optional chaining

console.log(fetchedUserData?.job?.title);

## Nullish coaslescing

const userInput = null;

const storedData = userInput ?? "DEFAULT"; 


# Generics

  function merge<T,U>(objA: T, objB: U): {
    return Object.assign(objA, objB);
  }

  const mergedObj = merge({name: "Max"}, {age: 30});

  console.log(mergedObj.name);


## Constraints in generics
//Instead of T and U freely, we can constraint them.

  function merge<T extends object,U extends object>(objA: T, objB: U): {
    return Object.assign(objA, objB);
  }

## Ensure generics have certain props

  interface Lenghty {
    length : number;
  }

  function countAndDescribe<T extends Lengthy>(element: T) : [T, string] {
    ...
  }

## Ensure that a generic is a key of the other generic
//T should be any kind of object and U should be any kind of key in that object


  function extractAndConvert<T extends object, U extends keyof T>(obj: T, key: U) {
    return "Value: " + obj[key];
  }

## Generic class
When you don't care of the type your affecting with your class but you want nonetheless type safety.

  class DataStorage<T> {
    private data: T[] = [];

    addItem(item: T){
      this.data.push(item);
    }

    removeItem(item: T){
      t..
    }

    getItems(){
      return [...this.data];
    }
  }


const textStorage = new DataStoreage<string>();
textStorage.addItem("Max");
textStorage.addItem("Manuel");

const numberStorage = new DataStorage<number>();
numberStorage.addItem(20);
...

## Partials
Allows us to describe an object of any class in which all its props are optional.

  inteface CourseGoal {
    title: string,
    description: string,
    completeUntil: Date;
  }

  function createCourseGoal(title: string description: string,date: Date ) : CourseGoal {
    let courseGoal : partial<CourseGoal> = {};
    courseGoal.title = title;
    courseGoal.description = description;
    CourseGoal.completeUntil = date;
    return courseGoal as CourseGoal;
  }

## Readonly
Allows us to put a cap on arrays for example, meaning they are only setable on initialization

  const names: Readonly<string[]> = ["Max","Manu"];
  names.push("Luís"); //this gives out error



# Decorators
First enable experimentalDecorators on the tsconfig.json

  function Logger(constructor: Function){
    console.log("Logging...");
    console.log(constructor);
  }

  @Logger
  class Person {
    name = "Max";

    constructor() {
      console.log("Creating person object...");
    }
  }

  const pers = new Person();

## Decorator Factory
returns a decorator function

  function Logger(logString: string){
    return function(constructor: Function){
      console.log(logString);
      console.log(constructor);
    }
  }

  @Logger("LOGGING - PERSON")
  class Person {
    name = "Max";

    constructor() {
      console.log("Creating person object...");
    }
  }

## Some useful Decorator
Make something appear in DOM where a certain Id appears

  function WithTemplate(template: string, hookId: string){
    return function(_: Function) {
      const hookElement = document.getElementById(hookId);
      if(hookElement){
        hookElement.innerHtml = template
      }
    }
  }

  @WithTemplate("<h1>My Person Object</h1>","app")
  class...


## Number and order of decorators
We can have more than 1 decorator, in that case, it runs bottom up.

  @Logger("");
  @WithTemplate();

In this case withtemplate decorator will run first and then the other.

## Decorate properties

  function Log(target: any, propertyName: string) {
    ...
  }

  class Product {
    @Log
    title: string;
    private _price: number;
  }

## Decorate Accessors

  function Log2(target: any, name: string, descriptor: PropertyDescritor) {
    ...
  }

  @Log2
  set prices(val: number){
    this._price = val;
  }
  
## Decorate methods

  function Log3(target: any, name: string, descriptor: PropertyDescriptor) {
    ...
  }

  @Log3
  MyMethod()...

## Decorate a param

  function Log4(target: any, name: string, position: number) {
    ...
  }

  MyMethod(@Log4 tax: number)


# Splitting Code
There are 2 ways to split our code
* Namespaces & File Bundling 
* Imports/Exports (Recommended approach)


## Namespaces & File Bundling

To use it we envelop the code with the namespace object and then export at the front of each class, interface etc

  namespace nameOfNameSpace {
    export interface MyInterface
  }

and then to call them on other files, on top of each file write:

  /// <reference path="nameOfTheFile.ts"/>

and include it in the same namespace by the method above

!IMPORTANT: For this to work we must go to the tsconfig.json and comment in the outFile option and module to "amd"



## Imports and Exports (Recommended Approach)

To use it, we add the keyword export in front of whatever want to export like the approach above, and then to call them on other files we use the keyword import {} from. 
!IMPORTANT: the location of the import must be .js as it was importing the already compiled file.

  export interface MyInterface

  import {Draggable} from ./models/drag-drop.js;

!IMPORTANT: Change back the tsconfig "module" field to "es2015" and comment out the "outFile" field;
On the index.hmtl, on the script tag, we remove the defer and then add:

  <script type="module" src="dist/app.js"></script>



### Bundling Imports
Import everything from a source an use an alias

  import * as Validation from ...


### Default export
In the export file we can use default export, meaning that if it's not named in the import, it will import the default.

  export default interface MyInterface

and then when importing

  import AnyName from ./..;

# Webpack
3rd party lib that packs all the js files to a bundle and reduces the footprint of the code to speed up requests.







