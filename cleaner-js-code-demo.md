# How to write a bit cleaner code for codebase / other devs

- Common practices to follow with your daily coding tasks ~

## Sample Examples

```js
// ###### 1. Avoid using nested if/else statements ~

// Not recommend version:
function processUser(user) {
  if (user != null) {
    if (user.hasSubscription) {
      if (user.age >= 18) {
        return showFullVersion();
      } else {
        return showChildrenVersion();
      }
    } else {
      throw new Error('User does not have a subscription');
    }
  } else {
    throw new Error('No User Found ..');
  }
}

// Recommend version:
function processUser(user) {
  if (!user) throw new Error('No User Found ..');

  if (!user.hasSubscription) throw new Error('User does not have a subscription');

  if (user.age >= 18) return showFullVersion();
  
  return showChildrenVersion();
}


// ###### 2. Using better naming for function and variables

// Not recommend version:
const MIN_PASSWORD = 6;

function passwordLength(password) {
  return password.length >= MIN_PASSWORD;
}

// Recommend version:
const MIN_PASSWORD_LENGTH = 6; // better constant naming which indicates the purpose of the function

function isPasswordLengthValid(password) { // better function name also good for readability FOR OTHER DEVS ~
  return password.length >= MIN_PASSWORD_LENGTH;
}


// ###### 3. Over commenting inside codebase

// Not recommend version:
// Function to check if a number is prime
function isPrime(number) {
  // Check if the number is less than 2
  if (number < 2) {
    // If less than 2, not a prime number
    return false;
  }

  // At least 1 divisor must but less than square root, so we can stop there
  for (let i = 2; i <= Math.sqrt(number); i++) {
    // Check if number is divisible by i
    if (i % number === 0) {
      // If divisible, number is not a prime number
      return false;
    }
  }

  // If none of the above conditions are met, the number is prime
  return true;
}

// Recommend version:
function isPrime(number) {
  if (number < 2) {
    return false;
  }

  // At least 1 divisor must but less than square root, so we can stop there
  for (let i = 2; i <= Math.sqrt(number); i++) {
    if (i % number === 0) {
      return false;
    }
  }

  return true;
} // we might only need the comment for the loop part which could be helpful for the readers to understand code a bit faster ~


// ###### 4. the coding format consistency consideration

// Not recommend version:
let name = "Damon";
const age=18;

function getUserInfo() {
  console.log("User info: ");
    console.log('Name: ' + name);
      console.log(`Age: ${age}`);
}

// Recommend version:
const nameDemo = "Damon"; // change to const because name not reassigned yet ~
const ageDemo = 18; // equal (=) symbol has spaces between

function getUserInfo() {
  console.log("User info: "); // the indentation consistency with all code inside the function
  console.log(`Name: ${nameDemo}`); // Using template literal statement ~
  console.log(`Age: ${ageDemo}`);
}


// ###### 5. Always keep code as DRY as possible !!

// Not recommend version:
function logLogin() {
  console.log(`User ${new Date().toISOString()} logged in.`);
}

function logLogout() {
  console.log(`User ${new Date().toISOString()} logged out.`);
}

function logSignUp() {
  console.log(`User ${new Date().toISOString()} signed up.`);
}

// Recommend version:
function logAction(actionName) {
  console.log(`User ${new Date().toISOString()} ${actionName}.`);
}
// Usage Example:
function logLogin() {
  logAction('logged in');
}


// ###### 6. Error first statement, also can reduce '?' symbol ~

// Not recommend version:
function getUppercaseInput(input) {
  const result = input?.toUpperCase()?.();

  if (typeof input !== 'string' || input.trim() === '') {
    throw new Error('Invalid input ~');
  }

  return result;
}

// Recommend version:
function getUppercaseInput(input) {
  if (typeof input !== 'string' || input.trim() === '') {
    throw new Error('Invalid input ~');
  }

  return input.toUpperCase();
}


// ###### 7. Make code more descriptive ~

// Not recommend version:
let price = 10;

if (transactionType === 1) {
  price *= 1.1;
}

// Recommend version:
const TRANSACTION_TYPE = 1;
const TRANSACTION_DISCOUNT_RATE = 1.1;

let transactionPrice = 10;

if (transactionType === TRANSACTION_TYPE) { // this is now more descriptive
  transactionPrice *= TRANSACTION_DISCOUNT_RATE;
} // as a reader, I could be more easier to understand this version of code, much clearer ~


// ###### 8. Make code appearing with as less side effects as possible

// Not recommend version:
let area = 0;

function calculateAndUpdateArea(radius) { // seems like this function has 2 responsibilities: calculate + update area
  const newArea = Math.PI * radius * radius;
  
  area = newArea; // this is side effect, assign area value within the function scope ..

  return newArea;
}

// Recommend version:
let areaDemo = 0;

function calculateArea(radius) { // Single responsibility principle: oly calculate the area value ~
  return Math.PI * radius * radius;
}

areaDemo = calculateArea(10); // update the areaDemo, which is separated and no side effect ! 


// ###### 9. More readable approach during coding

// Not recommend version:
const numbers = [...Array(10)].map((_, i) => i + 1);

const result = numbers.reduce((acc, num) => num & 1 ? [...acc, num * num] : acc, []);

console.log(result);

// Recommend version:
const readableResult = numbers.filter(n => n % 2 !== 0).map(n => n * n); // more readable for the code !!

console.log(readableResult);


// ###### 10. Making optimization before you really need optimization !!!!!!
// Not recommend version:
function countingSort(arr, min, max) {
  let count = new Array(max - min + 1).fill(0);

  arr.forEach((element) => {
    count[element - min]++;
  });

  let index = 0;

  for (let i = min; i <= max; i++) {
    while (count[i - min] > 0) {
      arr[index++] = i;
      count[i - min]--;
    }
  }

  return arr;
}

const arrDemo = [4, 2, 2, 8, 3, 3, 1];

console.log(countingSort(arrDemo, 1, 8));

// Recommend version:
const countingSortDemo = [4, 2, 2, 8, 3, 3, 1];
const sortedResult = countingSortDemo.sort(); // just use built-in APIs if possible, no need make customized complex code logics
// which makes devs life much easier !!!
console.log(sortedResult);
```
