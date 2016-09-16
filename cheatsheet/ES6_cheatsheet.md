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

validateShoppingList("Oranges", "Apples", "Honey");
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