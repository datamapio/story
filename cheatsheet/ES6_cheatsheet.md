# ES6 Cheatsheet
See also: https://babeljs.io/docs/learn-es2015/     
     
Most examples from: https://www.udemy.com/javascript-es6-tutorial/learn/v4/overview

## Array Helpers

### .map vs .forEach
Array.prototype.map returns an array, Array.prototype.forEach doesn't. 
Which means you can chain additional methods when using .map, but no so with .forEach.    
                   
See also: http://stackoverflow.com/questions/354909/is-there-a-difference-between-foreach-and-map 


### .filter
The reject function should result in the opposite of "number >= 10".
```
var numbers = [8, 9, 10, 11, 12];

function reject(array, iteratorFunction) {
  // first filter
  var res = array.filter(iteratorFunction);  // [10, 11, 12]
  // reverse filter using the result of the first filter
  return array.filter(function(element) { return res.indexOf(element) < 0}); // [8, 9]
}

var lessThanTen = reject(numbers, function(number){
  return number >= 10;
}); 

lessThanTen;
```
If indexOf is confusing, try this first:
```
function notInArray(el) { return [10,11,12].indexOf(el); }
function notInArray2(el) { return [10,11,12].indexOf(el) < 0; }
notInArray(8);
notInArray(11);
notInArray2(8);
notInArray2(11);
```


### .reduce
A reducer takes an accumulator value and a single value from a collection and computes a new accumulator value.      

In the first example:       
previous = previousValue or accumulator;     
trip = currentValue or single value, here an object, from the array;    
0 = initialValue        

In the second example:    
tally = previousValue or accumulator and     
vote = currentValue or single value from a collection. Together they form the reducer.        
                    

Note: The reduce array helper doesn't need an initialValue.     

Note 2: The name Redux comes from the Reducers:              
See: https://github.com/reactjs/redux/issues/514#issuecomment-131232439

```
var trips = [{ distance: 34 }, { distance: 12 } , { distance: 1 }];

var totalDistance = trips.reduce((previous, trip) => {
    return previous += trip.distance;
}, 0);
```

From [Egghead.io](https://egghead.io/lessons/javascript-introducing-reduce-reducing-an-array-into-an-object)
```
var votes = [
  "angular",
  "react",
  "react",
  "angular",
  "react",
  "angular",
  "react",
  "ember",
  "vanilla",
  "react"
];

var initialValue = {};

// tally = sum
var reducer = function(tally, vote) {
  if(!tally[vote]) {
    tally[vote] = 1;
  } else {
    tally[vote] = tally[vote] + 1;
  }
  return tally;
};

var result = votes.reduce(reducer, initialValue);
console.log(result);
```


## Rest and Spread Operator
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


## Destructuring
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
## Classes
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

## Generators
Generators, a new feature of ES6, are functions that can be paused and resumed (think cooperative multitasking or coroutines). That helps with many applications. 

Two important ones are:
- Implementing iterables
- Blocking on asynchronous function calls

Attention: Don't use generators and iterators for now. See 11.1 and 11.2     
https://github.com/airbnb/javascript#iterators-and-generators
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

### Generator Delegation   
```
const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill'
}

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  engineer: 'Dave',
  manager: 'Alex'
}

function* TeamIterator(team) {
  yield team.lead;
  yield team.engineer;
  const testingTeamGenerator = TestingTeamIterator(team.testingTeam);
  yield* testingTeamGenerator;
}

function* TestingTeamIterator(team) {
  yield team.lead;
  yield team.tester;
}

const names = [];
for (let name of TeamIterator(engineeringTeam)) {
  names.push(name);
}

names;
```
### Generator delegation, refactored with Symbol.iterator    

The Symbol.iterator well-known symbol specifies the default iterator for an object. Used by for...of.      
```
const testingTeam = {
  lead: 'Amanda',
  tester: 'Bill',
  [Symbol.iterator]: function* () {
    yield this.lead;
    yield this.tester;
  }
}

const engineeringTeam = {
  testingTeam,
  size: 3,
  department: 'Engineering',
  lead: 'Jill',
  engineer: 'Dave',
  manager: 'Alex',
  [Symbol.iterator]: function* () {
    yield this.lead;
    yield this.engineer;
    yield* this.testingTeam;
  }
}

const names = [];
for (let name of engineeringTeam) {
  names.push(name);
}

names;
```
## Promises
```
promise = new Promise( (resolve, reject) => {
  setTimeout(() => {
    resolve();
  }, 3000); 
  //reject();
});

promise
  .then(() => console.log("Finally!"))
  .then(() => console.log("I was also ran"))
  .catch(() => console.log("Not cool"));
  ```
### Fetch
Stephen talks about the shortcomings of the native fetch (no catch when there is a 404 status) and recommends to use libraries like Axios.
```
url = 'https://jsonplaceholder.typicode.com/posts/';

fetch(url)
  .then(response => response.json())
  .then(data => console.log(data));
```

See also:   
https://babeljs.io/docs/learn-es2015/