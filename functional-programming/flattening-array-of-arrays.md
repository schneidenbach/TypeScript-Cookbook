### Flattening an array of arrays

Say you have an array of arrays full of objects you want to flatten into one array:

```typescript
const nestedArrays: Person[][] = [
    [
        {firstName: "Andrew", lastName: "Smith"},
        {firstName: "Derek", lastName: "Maloney"},
    ],
    [
        {firstName: "Chris", lastName: "Cawlins"},
    ],
    [
        {firstName: "Susan", lastName: "Sarandon"},
    ]
]
```

You can combine the power of a new Array, the `concat` function, and the spread operator to create a new array with all of the objects contained within it:

```typescript
const flattenedArray = [].concat(...nestedArrays);
```

This is the same as:

```typescript
const flattenedArray: Person[] = [
    {firstName: "Andrew", lastName: "Smith"},
    {firstName: "Derek", lastName: "Maloney"},
    {firstName: "Chris", lastName: "Cawlins"},
    {firstName: "Susan", lastName: "Sarandon"},
]
```

### Is this type safe?

You may notice that `[].concat` provides no type safety - this is by design since arrays in JavaScript can contain objects of any type.  To make sure you're only getting arrays of Person back, you can cast your array to be of type `Person[]`to give you an extra layer of type safety:

```typescript
const flattenedArray = ([] as Person[]).concat(...nestedArrays);
```

### When to use this

* For flattening arrays of arrays only one level deep!

### When not to use this

While this syntax is convenient and easy to remember, flattening arrays of arrays of arrays doesn't work here - it's also slow on huge arrays.  For a more performant solution, check out [this StackOverflow answer](https://stackoverflow.com/a/39000004/3853934) for a similar answer in JavaScript - it does the same thing but is faster and works on deeply nested arrays.

