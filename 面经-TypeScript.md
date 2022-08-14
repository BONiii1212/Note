# TypeScript

⚠️非常重要

👑重要

⭐️一般

📎了解

## 📎TS和JS的区别

- TypeScript是JavaScript的超集
- TypeScript是具有静态类型的JavaScript
- TypeScript运行需要转换为JavaScript（一般使用ts-loader）

## 👑TypeScript数据类型

- number：数字
- string：字符串
- boolean：布尔类型
- array：所有元素值类型相同的集合
- tuple：数量固定，类型可以各异的集合，相较于object，解构赋值出来的名字可以进行重命名
- any：表示可以是任何值
- unknow：表示可以是任何值
- void：函数不返回值或者返回undefined
- never：表示永远不会返回结果（void也是一种返回值）
- enum：枚举类型
- null/undefined：既是一个值也是一个类型

## 👑类型守卫和类型断言

- **类型守卫**

  是TypeScript在运行时检测某个数据是否在所要的范围内

  ```tsx
  //添加类型守卫，检测是否是Error，有message就是Error
  const isError = (value: any): value is Error => value?.message;
  ```

- **类型断言**

  类型断言好比其他语言中的强制转换，但是又不进行特殊的数据检查和解构，仅仅是在编译阶段起作用，对运行时并没有作用（让编译器，听你的（首先你得是对的，不然在运行的时候会报错！！））

  ```tsx
  let someValue: any = "this is a string";
  //方法一
  let strLength1: number = (<string>someValue).length;
  //方法二
  let strLength2: number = (someValue as string).length;
  ```


## 📎const和readonly

首先const和readonly均可以实现不可变量

但是const所创建的引用类型，其内部的变量仍能进行改变（引用类型中存储的是地址，更改内部的变量，引用类型的地址确实没变）

```tsx
const a = [1, 2, 3];
a.push(102); //仍然能够进行更改，我们不希望是这样
```

因此就有readonly诞生惹

```tsx
//使数组中的元素值也无法被更改
let b: readonly number[] = [1, 2, 3];
b[0] = 101; //error

//直接使用ReadonlyArray<T>创建数组
let a: ReadonlyArray<number> = [1, 2, 3];
a.push(102); // error

//可以直接使用在类中指定变量身上
interface Rx {
  readonly x: number;
}
let rx: Rx = { x: 1 };
rx.x = 12; // error

//Readony<T>
interface X {
  x: number;
}
let rx: Readonly<X> = { x: 1 };
rx.x = 12; // error
```

## 📎枚举/完全嵌入枚举

**枚举类型**

允许我们定义一组命名常量

```js
/* 定义enum枚举 */
//数字枚举
enum Gender {
    Male = 1,
    Female = 2
}
//字符串枚举
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
let m: { name: string, gender: Gender }
m = {
    name: '张三',
    gender: Gender.Male
}
```

枚举会被编译成一个对象，可以当作对象使用

**完全嵌入枚举**

```js
const enum Color {
  Red,
  Green,
  Blue
}
//编译之后会变成var sisterAn = 0（被当成了Number来使用，如果不考虑编译之后的可读性，可以使用完全嵌入枚举来提高性能）
var sisterAn = Color.Red
```

## ⚠️type和interface区别

- **type：类型别名**

  ```
  type FavoriteNumber = string|number
  type AuthForm {
  	username: string;
    password: string;
  }
  ```

- **interface：接口**

  ```
  interface AuthForm {
    username: string;
    password: string;
  }
  ```

type和interface很多情况下可以互相转换，

但是在｜、&和Utility Type中type具有无法替代性

interface可以实现继承，type不行

## ⚠️any和unknown的区别

any和unknown均表示任何值

但是any是让TS将该对象当成JS的方式来处理

而unknown不允许赋值给其他类型，如果有必要必须进行断言

## 📎never和void的区别

never表示永远不存在的类型，常用于函数返回类型，表示永远都没有返回结果。

void表示空类型（函数返回设置成never时，无法返回void类型，因为void也是一种类型），只能赋予undefined或者null

## ⚠️as const

const断言告诉编译器为表达式推断出它能推断出的最窄或最特定的类型。如果不使用它，编译器将使用其默认类型推断行为，这可能会导致更广泛或更一般的类型。

## ⚠️TS鸭子类型

**继承**

TS中类型的继承，父辈能够接收子辈类型的值

鸭子类型（duck typing）：**面向接口编程**而不是**面向对象编程**

```tsx
interface Base{
  id:number
}
interface Advance extends Base{
  name:string
}
const test = (p:Base)=>{
}
const a={id:1,name:'jack'}
//不会报错，因为我需要id属性，只要你传的带id属性，那么就是合理的，并不是一样要类型符合，这就是鸭子类型
test(a)
```

