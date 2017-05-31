### Get the return type of a function

Say you have a function that returns a POJO with some methods and properties, like so:

```typescript
function getService(httpService: HttpService) {
    return {
        findFriends(): Promise<string[]> { },
        createFriend(name: string) : Promise<number> { },
        baseUrl: "https://contoso.com/api"
    }
}
```

You may want to define an interface that adheres to the "shape" of this object, so that you can safely use that type in other places. You may also have hundreds of functions like this one that you don't want to write hundreds of interfaces for!

Why do that when you can just get the return type like so:

```typescript
const getServiceDummy = (false as true) && getService(undefined);    //get an "instance" of the function
type MyService = typeof getServiceDummy;                             //create a type from that instance
```

You can then sprinkle liberally throughout your application to get type safety in places you might not otherwise get it, such as Angular controller definitions:

```typescript
function AppCtrl(service: MyService) {}
```

### When is this useful?

* Useful for getting the "shape" of a function's return value when you don't want/need to declare an interface for that object.
* When refactoring an existing Angular application to use TypeScript, this is especially useful when declaring controllers as you can then use the type inside of the controller function parameters.  It's not usually worth the overhead of creating type definitions for every single one of your internal services, so this shortcut can save you some time and type.

### Why does this work?

The `false as true `thing will definitely throw some people off - for me it sure did the first time I looked at it.  However, it makes total sense once you know how the TypeScript compiler interprets the value of `getServiceDummy`.

If you declared `getServiceDummy` like so, without the `false as true:`

```typescript
const getServiceDummy = false && getService(undefined);
```

...the TypeScript compiler is intelligent enough to know that the type of `getServiceDummy` is `boolean`, because it sees the false and short circuits - it doesn't even look at the value of `getService`as TypeScript knows it would never actually be called anyways.

If we made it `true && getService(undefined)`, we'd end up actually calling the function at runtime, which we don't want.

`(false as true)` gives us the best of both worlds - the function won't be called at runtime and the cast to `true` tricks the TypeScript compiler into thinking that the variable will have the value of the function return value.

