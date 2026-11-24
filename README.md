# For Of 4 Lyfe

[![Tests](https://github.com/RickRieger/for-of-4-lyfe/actions/workflows/tests.yml/badge.svg)](https://github.com/RickRieger/for-of-4-lyfe/actions/workflows/tests.yml)

**No live site.** This exercise is a set of Jest tests, not a web page. The badge above shows the latest run on GitHub Actions; click it to see the results.

I did this exercise at the **Code Immersives** bootcamp in New York City in **March 2021**. It was forked from the instructor's repo. My code is at the top of [`main.test.js`](main.test.js), above the "OUR CODE BELOW; DO NOT TOUCH!" line. The instructor's original instructions are kept unchanged under [Original assignment](#original-assignment-from-the-instructor) below.

## What it practices

Every challenge uses a `for...of` loop. It visits each item of an array (or each character of a string) without needing an index:

```js
for (const num of nums) {
  total += num;
}
```

The eight challenges fall into three patterns. Later in the course these become the built-in array methods `reduce`, `map` and `filter`, so my code is grouped that way:

| Pattern | What comes out | Challenges |
| --- | --- | --- |
| **Reduce** | many values in, **one** value out | `addAll`, `countSpaces`, `countTrues`, `makeFriendly` |
| **Map** | the **same number** of items, each one changed | `cubeAll`, `addNoises` |
| **Filter** | the **same number or fewer** items, none changed | `womenOnly`, `integersOnly` |

## My solution

- **addAll** keeps a running `total`.
- **countSpaces** counts characters that are exactly `' '`, so commas and `@` don't count.
- **countTrues** checks `value === true`, so truthy values like `'true'`, `3` and `[]` aren't counted.
- **makeFriendly** builds a new string one character at a time, swapping each `.` for `!`.
- **cubeAll** pushes `num ** 3` into a new array, so the original isn't changed.
- **addNoises** adds "Woof!", "Meow!" or "ROWR." for the dogs, cats and dinosaurs named in the tests. An animal it doesn't know is kept as-is (see the cleanup log).
- **womenOnly** keeps the strings that end in `F`.
- **integersOnly** keeps the numbers where `Number.isInteger(num)` is true.

## Run the tests

```bash
npm ci
npm run test:ci   # runs once and exits (expected: Tests: 18 passed, 18 total)
npm test          # the instructor's watch mode: re-runs on every save, press q to quit
```

## How it's checked

- **GitHub Actions** ([`.github/workflows/tests.yml`](.github/workflows/tests.yml)) runs `npm ci` and `npm run test:ci` on Node 20 on every push and pull request. The badge at the top shows the result.
- The 17 original tests are unchanged. I added **one test** for `addNoises` with an animal that isn't on the list, because the original tests only used known animals.

## Cleanup log

### 2026-11-24

**1. `addNoises` dropped animals it didn't know**

*Why:* `addNoises` is a map, so it should return one item for every item it's given. My original code only pushed known animals: `['Fido', 'Bob', 'Garfield']` came back with **2** items and Bob was silently lost. The tests never tried an unknown animal, so they passed anyway. Now an unknown animal is kept by name. A new test (`addNoises (unknown animals)`) checks this, and it fails on the old code.

Before:

```js
else if(value === 'Sharp Tooth'|| value === 'Barnie'){
  newValue = value.concat(' says "ROWR."');
  newArray.push(newValue);
}
// (no else: any other animal was skipped)
```

After:

```js
} else if (animal === 'Sharp Tooth' || animal === 'Barnie') {
  withNoises.push(`${animal} says "ROWR."`);
} else {
  // unknown animal: keep its name so the array stays the same length (it's a map)
  withNoises.push(animal);
}
```

**2. Tidied the code section**

*Why:* to make the code easier to read without changing what it does. I ran the old and new versions of every function on the same 21 inputs and got the same results. (For `addNoises` I used only known animals, since item 1 changed it on purpose.)

- Grouped the functions under `Reduce`, `Map` and `Filter` comments.
- `makeFriendly` called `value.replace('.', '!')` on every single character. That works, but it runs a string search on one letter at a time. A plain comparison says what it means.
- Removed the unused `newValue` variable in `womenOnly` and the extra `newValue` steps in `cubeAll` and `addNoises`.
- Used descriptive names (`num`, `char`, `animal`, `cubes`, `women`) instead of `value` and `newArray` everywhere.
- Used template literals instead of `.concat`, and `num ** 3` instead of `Math.pow`. Wrote `total++` consistently instead of mixing `total = total +1` and `total ++`.
- Fixed the uneven indentation.

Before:

```js
function makeFriendly(string){

  let newString = '';
  for (const value of string){
  newString += value.replace('.', '!');
  }
  return newString;
}

function cubeAll(array){

  let newArray = [];
  let newValue = 0;

  for (const value of array){
    newValue = Math.pow(value, 3);
    newArray.push(newValue);
  }

  return newArray;
}
```

After:

```js
function makeFriendly(string) {
  let newString = '';
  for (const char of string) {
    if (char === '.') {
      newString += '!';
    } else {
      newString += char;
    }
  }
  return newString;
}

function cubeAll(nums) {
  const cubes = [];
  for (const num of nums) {
    cubes.push(num ** 3);
  }
  return cubes;
}
```

**3. Tests run automatically on GitHub**

*Why:* the instructor's `npm test` runs Jest in watch mode, which never exits, so it can't run on a server. I added a one-shot script, the Actions workflow, and the badge at the top.

Before (`package.json`):

```json
"scripts": {
  "test": "jest --watch-all"
}
```

After:

```json
"scripts": {
  "test": "jest --watch-all",
  "test:ci": "jest --ci"
}
```

**4. This README**

*Why:* the repo only had the instructor's instructions. I added this section above them: what the exercise practices, my solution, how to run and check it, and this log.

---

## Original assignment (from the instructor)

### For Of 4 Lyfe

### Introduction

This will be an exercise in using `for of` loops.

Most times you iterate through an array, you don't need the index you're currently on, so `for of` loops are the simpler and nicer-looking choice.

Together, they make a dynamic duo, serving up simpler and safer syntax for solution-slingers!

That one got away from me. Sorry.


### Setup

1. Fork and clone.
2. Navigate into the directory.
3. Run `jest --watch-all`.
4. Code away in `main.test.js`, NOT `main.js`. (There isn't one for annoying technical test-related reasons.)
5. Read the tests further down in `main.test.js` for function usage examples!


### Guidelines

* Although we won't be testing for what kind of loop you'll be using, this isn't just an exercise in solving problems. Let's use our new `for of` loop!

### Challenge Descriptions

##### General Looping Problems

* `addAll` - given an array of numbers, returns the sum of all numbers added together. **HINT:** this is a reduce problem, reducing a collection of values to one value. In this case, a collection of numbers to one total sum.
* `countSpaces` - given a string, returns the number of characters that are spaces. **HINT:** this is a reduce problem, reducing a collection of values to one value.
* `countTrues` - given an array of booleans, returns the number of `true` values. **HINT:** this is a reduce problem, reducing a collection of values to one value.
* `makeFriendly` - given a paragraph, returns a paragraph where each sentence ends with an exclamation point instead of the period it held before. **HINT:** this is a string-building problem, and you'll want to mostly append to the string you'll return each character, except when you hit a period.


**Maps**

##### A Helpful Reminder

A map:

* returns the same number of items as in the original collection
* with some items transformed

##### Exercises

* `cubeAll` - given an array of numbers, returns an array where each number has been cubed (a.k.a. raised to the third power).
* `addNoises` - given an array of strings, returns an array where each string has appended to it that animal's noise. See tests for examples! **HINT:** You're definitely going to have to check for each tested-for animal name, but you could use a compound boolean in your `if` check, or set up an array of your own animals and use `.includes`, or use a `switch` statement. Either way, you can look at the tests to see which names should be considered which animals!


**Filters**

##### A Helpful Reminder

A filter:

* returns the same number of items or fewer (usually fewer!)
* with NO items transformed

##### Exercises

* `womenOnly` - given an array of strings with names and gender, returns an array of only those strings where the gender (the last character in the string) is 'F'. **HINT:** you can check the character at last index of the string to see if it's an `F`, but there's also a string method that tells you if a string "ends with" another string. [Duck it!](https://duckduckgo.com/)
* `integersOnly` - given an array of numbers, returns an array of only those numbers that are integers. **HINT:** you can use modulus for htis, it's just SEEMS weird to do so.
