#ES6 Cheatsheet
Examples from: https://www.udemy.com/javascript-es6-tutorial/learn/v4/overview


##Rest and Spread Operator
```
// Rest Operator
function validateShoppingList(...items) {
  if (items.indexOf('Milk') < 0) {
    // Spread Operator  
	return [...items, 'Milk']; 
  }
  return items;
}

validateShoppingList('Oranges', 'Apples', 'Honey');
```
```
function addNumber(...numbers) {
  return numbers.reduce((sum, number) => {
		return sum + number;
  }, 0);
}

addNumber(4, 5, 6, 7, 8);
```
Flattening arrays with the spread operator
```
const redOrangeToOrange = ['#f2371b','#f23b17','#f14110','#f1450a','#f04a00','#f44d0b','#f75114','#fb541c','#ff5722'];
const whiteOrangeToBlack = ['#ffefe8','#e3d4ce','#c6bab5','#aba09c','#908884','#76706c','#5e5956','#464341','#302e2d','#1c1a1a','#000000'];

// Flattening arrays with the spread operator
const combinedColors = [...redOrangeToOrange, ...whiteOrangeToBlack];
combinedColors;
```
Rest: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters     
Spread: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator


##Destructuring
Example
```
var expense = {
	type: 'Business',
	amount: '$45 USD'
};

1. 
//ES5 
//var type = expense.type;
//var ammount = expense.amount;

2. 
//ES6
//const { type } = expense; // not an object on the left
//const { amount } = expense;

3.
const { type, amount } = expense; // variable must be identical to the property
type;
amount;

```
Example: properties as arguments, only use properties with the object name
```
const savedFile = {
	name: "myfile",
	extension: "jpg",
	size: 14400	
};

// instead of fileSummary(file) and then return `The ${file.name} file...

function fileSummary({ name, extension, size}, {color}) {
	return `The ${color} file ${name}.${extension} has the size of ${size}.`;
}

fileSummary(savedFile, {color:'red'});

```
Destructuring Arrays
```
const companies = ['Datamap', 'Kaywa', 'Uber'];

// Needs array or square brackets for elements, because we don't want a property. 
// Replaces const firstCompany = companies[0]; etc.

const [ name, name1, name2 ] = companies;
name;
name1;

// With curly braces/brackets, we ask for a property
const { length } = companies;
length;

// with the rest operator, we get back the rest 
const [ firstName, ...rest ] = companies;
rest;

```
Array with Objects
```
const companies = [
  { name: "Google", location: "Mountain View"},
  { name: "Facebook", location: "Menlo Park"},
  { name: "Uber", location: "San Francisco"}
];

const [ location ] = companies; // {"name":"Google","location":"Mountain View"}
const [{ location }] = companies; // Mountain View

```
Object with Array
```
const Kaywa = {
	locations: ["San Francisco", "Waedenswil", "Belgrade"]
};

const { locations: locations } = Kaywa;
locations; // ["San Francisco","Waedenswil","Belgrade"]

// Get the first element in the Array
const { locations: [ location ] } = Kaywa;
location; // San Francisco
```
Turn a list of arrays into a list of objects     
Problem:

```
// API data structure
const points = [
  [4, 5],
  [0, 3],
  [20, 1]
];

// Data structure needed within the graphic
[
  {x: 4, y: 5},
  {x: 0, y: 3},
  {x: 20, y: 1}
];

```
Solution:
```
// API data structure
const points = [
  [4, 5],
  [0, 3],
  [20, 1]
];

points.map(([ x, y ]) => {
	return { x, y };
});

```
Another example:
```
const classes = [
  [ 'Chemistry', '9AM', 'Mr. Darnick' ],
  [ 'Physics', '10:15AM', 'Mrs. Lithun'],
  [ 'Math', '11:30AM', 'Mrs. Vitalis' ]
];

const classesAsObject = classes.map(([subject, time, teacher]) => {
    return { subject, time, teacher };    
});

classesAsObject;
```
##Classes
```
class Car {
  constructor({ title }) {
    this.title = title;
  }

  honk() {
    return "beep";
  }

}

class Tesla extends Car {
  constructor(options) {
    super(options);
    this.color = options.color;
  }

  drive() {
   return "shhhh";
  }
}


const tesla = new Tesla({ color: "blue", title: "The future"});
tesla;
tesla.honk();
tesla.drive();
```
See also: https://scotch.io/tutorials/better-javascript-with-es6-pt-ii-a-deep-dive-into-classes

##Generators
Generators, a new feature of ES6, are functions that can be paused and resumed (think cooperative multitasking or coroutines). That helps with many applications. 

Two important ones are:
- Implementing iterables
- Blocking on asynchronous function calls
```
function* colors() {
  yield "red";
  yield "blue";
  yield "yellow";
  yield "green";
}

const myColors = [];
for (let color of colors()) {
  myColors.push(color);
}

myColors; //["red", "blue", "yellow", "green"]
```
See also: http://exploringjs.com/es6/ch_generators.html#_overview-16
```
const engineeringTeam = {
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  engineer: 'Dave',
  manager: 'Alex'
}

function* teamIterator(team) {
  yield team.lead;
  yield team.engineer;
}

const names = [];
for (let name of teamIterator(engineeringTeam)) {
  names.push(name);
}
```
