# typscript-types

一些常见的 TypeScript 类型

## Pick

从类型 T 中选择出属性 K，构造成一个新的类型。

```ts
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

## Readonly

该 `Readonly` 会接收一个 _泛型参数_，并返回一个完全一样的类型，只是所有属性都会被 `readonly` 所修饰。

```ts
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

## TupleToObject

传入一个元组类型，将这个元组类型转换为对象类型，这个对象类型的键/值都是从元组中遍历出来。

```ts
type TupleToObject<T extends readonly any[]> = {
  [P in T[number]]: P;
};

// 例如：
const tuple = ["tesla", "model 3", "model X", "model Y"] as const;
const result: TupleToObject<typeof tuple>; // expected { tesla: 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}
```

## First

一个通用 `First<T>`，它接受一个数组 `T` 并返回它的第一个元素的类型。如果数组是空，则无返回。

```ts
type First<T extends any[]> = T["length"] extends 0 ? never : T[0];
// 或者
type First<T extends any[]> = T extends [infer F, ...infer M] ? F : never;
```

## Last

它接受一个数组 T 并返回其最后一个元素的类型。

```ts
type Last<T extends any[]> = T extends [...infer M, infer L] ? L : never;
```

## Length

该 `Length` 接受一个 `readonly` 的数组，返回这个数组的长度。

```ts
type Length<T extends readonly any[]> = T["length"];
```

## Exclude

从联合类型 T 中排除 U 的类型成员，来构造一个新的类型。

```ts
type Exclude<T, U> = T extends U ? never : T;
```

## Awaited

假如我们有一个 Promise 对象，这个 Promise 对象会返回一个类型。在 TS 中，我们用 Promise 中的 T 来描述这个 Promise 返回的类型。请你实现一个类型，可以获取这个 T 。

```ts
type Awaited<T extends Promise<any>> = T extends Promise<infer R> ? R : never;
```

## If

接收一个条件类型 C ，一个判断为真时的返回类型 T ，以及一个判断为假时的返回类型 F。 C 只能是 true 或者 false， T 和 F 可以是任意类型。

```ts
type If<C extends boolean, T, F> = C extends true ? T : F;
```

## Concat

在类型系统里实现 JavaScript 内置的 Array.concat 方法，这个类型接受两个参数，返回的新数组类型应该按照输入参数从左到右的顺序合并为一个新的数组。

```ts
type Concat<T extends any[], U extends any[]> = [...T, ...U];
```

## Includes

在类型系统里实现 JavaScript 的 Array.includes 方法，这个类型接受两个参数，返回的类型要么是 true 要么是 false。

```ts
type Includes<T extends readonly any[], U> = T extends [infer F, ...infer M]
  ? Equal<F, U> extends true
    ? true
    : Includes<M, U>
  : false;
```

## Push

在类型系统里实现通用的 Array.push

```ts
type Push<T extends any[], U> = [...T, U];
```

## Unshift

实现类型版本的 Array.unshift

```ts
type Unshift<T extends any[], U> = [U, ...T];
```

## Parameters

```ts
type Parameters<T extends (...args: any[]) => any> = T extends (
  ...args: infer R
) => any
  ? R
  : never;
```

## ReturnType

```ts
type ReturnType<T extends (...args: any[]) => any> = T extends (
  ...args: any[]
) => infer R
  ? R
  : never;
```

## Omit

Omit 会创建一个省略 K 中字段的 T 对象

```ts
type Omit<T, K extends keyof T> = {
  [P in Exclude<keyof T, K>]: T[P];
};
```
