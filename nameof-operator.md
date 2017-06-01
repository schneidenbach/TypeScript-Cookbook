### nameof operator

Want to guarantee type safety of "stringly typed" property names in your code?

The TypeScript language doesn't include a `nameof` operator like in C\#, but you can make one yourself easily:

```typescript
const nameof = <T>(name: keyof T) => name;
```

All this does is take a type and a string and return the string:

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

const personName = nameof<Person>("firstName");    //returns "firstName"
```

That's not the best part though!  The best part isl, if you give it a string that isn't a key on that object, the compiler will give you an error:

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

const personName = nameof<Person>("noName");    //error!
```

### nameof Factory

I like to declare one generic `nameof` factory function and use it throughout my app, like so:

```typescript
//nameofFactory.ts

export const nameofFactory = <T>() => (name: keyof T) => name;
```

```typescript
import nameofFactory from './nameofFactory';

const nameof = nameofFactory<Person>();
```

### Why is this useful?

* **Type safety!**  TypeScript is all about making JavaScript scale intelligently.  `nameof` is just one of the tricks in the book that makes life a little easier when you want the type safety of knowing that the string you type is a property on a given object.
* When declaring React components with inputs bound to the names of properties on an object, you can use `nameof` to guarantee that the property name will remain the same after you change it somewhere else:

```typescript
import nameofFactory from './nameofFactory';

const nameof = nameofFactory<Person>();

export const personForm = () =>
    <div>
        First Name: <input name={nameof("firstName")} />
        Last Name: <input name={nameof("lastName")} />
    </div>
```

If the names of `firstName` or `lastName` ever change, TypeScript will error on compile.

