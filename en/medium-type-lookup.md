<h1>Type Lookup <img src="https://img.shields.io/badge/-medium-eaa648" alt="medium"/> <img src="https://img.shields.io/badge/-%23union-999" alt="#union"/> <img src="https://img.shields.io/badge/-%23map-999" alt="#map"/></h1>

## Challenge

Sometimes, you may want to lookup for a type in a union to by their attributes. 

In this challenge, we would like to get the corresponding type by searching for the common `type` field in the union `Cat | Dog`. In other words, we will expect to get `Dog` for `LookUp<Dog | Cat, 'dog'>` and `Cat` for `LookUp<Dog | Cat, 'cat'>` in the following example.

```ts
interface Cat {
  type: 'cat'
  breeds: 'Abyssinian' | 'Shorthair' | 'Curl' | 'Bengal'
}

interface Dog {
  type: 'dog'
  breeds: 'Hound' | 'Brittany' | 'Bulldog' | 'Boxer'
  color: 'brown' | 'white' | 'black'
}

type MyDogType = LookUp<Cat | Dog, 'dog'> // expected to be `Dog`
```

## Solution

At first, I thought it will be a huge solution with a lot of explanations, but turns out nothing out of ordinary.

We know about [Conditional Types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#conditional-types) in TypeScript and we know that we can use it to check if the type is assignable to some specific layout (if I may say so).

Let us check if `U` is assignable to the `{ type: T }` then:

```ts
type LookUp<U, T> = U extends { type: T } ? U : never;
```

BTW, worth noting that Conditional Types in TypeScript are [distributive](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#distributive-conditional-types).
So that each item from the union is going to be checked against our condition.