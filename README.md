# Build a JS functional library

## Project guidelines

Your assignment today is to build the `fi` JS library. This is a toolset of useful functional programming helpers, following the [functional programming](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0) paradigm.

_NOTE_ You should try to first complete this assignment by testing your functions with the example inputs found in this readme. Open `index.html` in the browser and try to use Chrome's debugging tools. When you've finished, feel free to run `learn` from your terminal. Try doing this using BDD first, then run the tests.

Your functions should conform to the following guidelines:

1. Write pure functions
2. Avoid sharing or mutating state
3. Avoid side effects

Given the same input your functions should always return the same value.

## Instructions

Below you will find a list of function descriptions detailing what their name, parameters and return value should be. Your job is to develop the code to implement these functions.

The entire fi library should be wrapped in an [Immediately Invoked Function Expression](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression) (IIFE), like the example below.

```javascript
fi = (function() {
  return {
    libraryMethod: function() {
      return "Start by reading https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0";
    },

    each: function() {
      /*TODO*/
    }
  };
})();

fi.libraryMethod();
```

More info on the [Module Pattern](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript)

The point of this exercise is to build your own implementation of [Higher Order Functions](https://eloquentjavascript.net/05_higher_order.html) using lower level building blocks of the JavaScript language. So, your implementation of `map` should not use the native JavaScript `map`. Your version should use a `for` loop, `while` loop, or `for in` loop to produce the functionality described.

## Collection Functions (Arrays or Objects)

**fi.each**

`fi.each(collection, callback)`

Iterates over a **collection** of elements, passing each element in turn to a **callback** function. Each invocation of **callback** is called with one argument. If **collection** is an array, **callback**'s argument will be a value within the array. If **collection** is a JavaScript object, **callback**'s argument will be a value within the object (keys are not passed into **callback**). **Returns the original collection for chaining.**

```javascript
fi.each([1, 2, 3], alert);
=> alerts each number in turn and returns the original collection
fi.each({one: 1, two: 2, three: 3}, alert);
=> alerts each number value in turn and returns the original collection
```

**fi.map**

`fi.map(collection, callback)`

Produces a new array of values by mapping each value in **collection** through a transformation function (**callback**). The callback is passed one argument. **Returns a new array for chaining without modifying the original.**

```javascript
fi.map([1, 2, 3], function(num){ return num * 3; });
=> [3, 6, 9]
fi.map({one: 1, two: 2, three: 3}, function(num, key){ return num * 3; });
=> [3, 6, 9]
```

**fi.reduce**

`fi.reduce(collection, callback, [acc])`

Reduce boils down a **collection** of values into a single value. **Acc** (short for accumulator) starts as the initial state of the reduction, and with each successive step it should be accumulate the return value of **callback**. If there is no accumulator, the initial state is represented by the first value in the array or object. The callback is passed three arguments: the acc, the current value in our iteration (the element in the array or object), and finally a reference to the entire collection.

```javascript
fi.reduce([4,3], function(acc, val, collection) { return acc / val; }, 12);
=> 1
fi.reduce([1, 2, 3], function(acc, val, collection) { return acc + val; });
=> 6
```

**fi.find**

`fi.find(collection, predicate)`

Looks through each value in the **collection**, returning the first one that passes a truth test (**predicate**), or undefined if no value passes the test. The function returns as soon as it finds an acceptable element, and doesn't traverse the entire collection.

```javascript
fi.find([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; });
=> 2
```

**fi.filter**

`fi.filter(collection, predicate)`

Looks through each value in the **collection**, returning an array of all the values that pass a truth test (**predicate**).

```javascript
fi.filter([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; });
=> [2, 4, 6]
```

**fi.size**

`fi.size(collection)`

Return the number of values in the **collection**.

```javascript
fi.size({one: 1, two: 2, three: 3});
=> 3
```

## Array Functions

**fi.first**

`fi.first(array, [n])`

Returns the first element of an **array**. Passing **n** will return the first **n** elements of the array.

```javascript
fi.first([5, 4, 3, 2, 1]);
=> 5

fi.first([5, 4, 3, 2, 1], 3);
=> [5, 4, 3]
```

**fi.last**

`fi.last(array, [n])`

Returns the last element of an **array**. Passing **n** will return the last **n** elements of the array.

```javascript
fi.last([5, 4, 3, 2, 1]);
=> 1
```

**fi.compact**

`fi.compact(array)`

Returns a copy of the **array** with all falsy values removed. In JavaScript, _false_, _null_, _0_, _""_, _undefined_ and _NaN_ are all falsy.

```javascript
fi.compact([0, 1, false, 2, '', 3]);
=> [1, 2, 3]
```

**fi.sortBy**

`fi.sortBy(array, callback)`

Returns a sorted copy of **array**, ranked in ascending order by the results of running each value through **callback**. The values from the original array should be retained within the sorted copy, just in ascending order.  

_The point of this exercise is not to write your own sorting algorithm and you are free to use the native [JS sort](https://www.w3schools.com/js/js_array_sort.asp)_

_If you would like to go deeper and try to construct your own sorting algorithm this is a great extension. [Here](http://blog.benoitvallon.com/sorting-algorithms-in-javascript/sorting-algorithms-in-javascript-all-the-code/) is a list of sorting algorithms implemented in JS with additional resources_

```javascript
fi.sortBy([1, 2, 3, 4, 5, 6], function(num){ return Math.sin(num) });
=> [5, 4, 6, 3, 1, 2];
```

**fi.flatten (bonus function)**

`fi.flatten(array, [shallow])`
Flattens a nested **array** (the nesting can be to any depth).

If you pass **true** for the second argument, the array will only be flattened a single level.

```javascript
fi.flatten([1, [2], [3, [[4]]]]);
=> [1, 2, 3, 4];

fi.flatten([1, [2], [3, [[4]]]], true);
=> [1, 2, 3, [[4]]];
```

**fi.uniq**

`fi.uniq(array, [isSorted], [callback])`

Produces a duplicate-free version of the **array**, using _===_ to test object equality. In particular only the first occurrence of each value is kept. If you know in advance that the **array** is sorted, passing _true_ for **isSorted** will run a much faster algorithm. If you want to compute unique items based on a transformation, pass a **callback** function.

Specifically, if the callback function returns the same value that a previous execution of the callback also returned, (even if the original array's elements are different), we don't include it in the return array.

```javascript
fi.uniq([1, 2, 1, 4, 1, 3]);
=> [1, 2, 4, 3]

fi.uniq([1, 2, 3, 6], false, (x => x % 3));
=> [1, 2, 3]
```

## Object Functions

**fi.keys**

`fi.keys(object)`

Retrieve all the names of the **object**'s own enumerable properties.

```javascript
fi.keys({one: 1, two: 2, three: 3});
=> ["one", "two", "three"]
```

**fi.values**

`fi.values(object)`
Return all of the values of the **object**'s own properties.

```javascript
fi.values({one: 1, two: 2, three: 3});
=> [1, 2, 3]
```

**fi.functions**

`fi.functions(object)`

Returns a sorted collection of the names of every function in an object — that is to say, the name of every property whose value is a function.

```javascript
fi.functions(fi);
=> ["compact", "each", "filter", "find", "first", "functions", "last", "map", "reduce", "size", "sortBy"]
```

**fi.giveMeMore**

If you are reading this come to us for more functions assignments.
