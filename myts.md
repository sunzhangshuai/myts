# TypeScript

> 比js更严格的语言，新增了类型校验

安装TypeScript

- 通过npm（Node.js包管理器）

  ```shell
  npm install -g typescript
  ```

- 安装Visual Studio的TypeScript插件

  Visual Studio 2017和Visual Studio 2015 Update 3默认包含了TypeScript。 

## 基础类型

### number

> 浮点数

```ts
let num:number = 12
```

### boolean：false/true

```ts
let bool:boolean = true;
```

### string

- 普通字符串：`""`，`'' `

  ```ts
  let str:string = "1234"
  ```

- 模板字符串：`` ，  ${表达式}

  ```ts
  let str2:string = `1234${str}`
  ```

### 数组 Array

- 定义

  - 第一种，可以在元素类型后面接上`[]`

    ```ts
    let arr2:number[] = [1,2,3,4,5];
    ```

  - 第二种方式是使用数组泛型，`Array<元素类型>`

    ```ts
    let arr1:Array<number> = [1,2,3,4,5];
    ```

### 元组 Tuple

> 元组类型允许表示一个已知**元素数量**和**类型**的**数组**，各元素的类型不必相同。

```ts
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

- 当访问越界元素时，会使用联合类型替代（即string, number都可以）

  ```
  x[3] = 12; // OK
  x[5] = true; // Error, 布尔不是(string | number)类型
  ```

- 当访问一个已知索引的元素，会判断该元素的类型

  ```ts
  x[1].substr() // error, 'number' does not have 'substr'
  x[0].substr() // OK
  ```

### 枚举 enum

> 是对JavaScript标准数据类型的一个补充
>
> 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字
>
> - 初始值默认为 0，其余的属性按顺序依次递增；
>
> - 手动给某个属性赋值(从任意值开始)，其余的属性依旧按顺序递增
> - 都采用手动赋值(常量/字符串枚举)

- 普通枚举

  - 默认情况

    ```ts
    enum Color = {rad, green, blue} // 初始值默认为 0
    let c: Color = Color.green // 1
    ```

  - 手动设置初始值，其余的属性依旧按顺序递增

    ```ts
    enum Color = {rad = 4, green, blue}
    let c: Color = Color.green // 5
    ```

  - 都采用手动赋值

    ```ts
    enum Color = {rad = 1, green = 4, blue = 10}
    let c: Color = Color.green // 4
    ```

- 字符串枚举

  ```ts
  enum Color {     
      Red='红色',
      Blue='蓝色',
      Green='绿色'
  }
  console.log(Color.Blue);  //蓝色
  ```

- 常量枚举：使用 const 关键字修饰的枚举

  ```ts
  const enum Color {
      Red,
      Blue,
      Green
  }
  console.log(Color.Red,Color.Blue,Color.Green);  //0 1 2
  ```

### any

> 在编程阶段还不清楚类型的变量指定一个类型，可以是任意类型

- 数组

  ```ts
  let list: any[] = [1, true, "free"];
  list[1] = 100;
  ```

- 变量

  ```ts
  let notSure: any = 4;
  notSure = "maybe a string instead";
  notSure = false; // okay, definitely a boolean
  ```

### void 没有类型

> `void`类型像是与`any`类型相反，它表示没有任何类型

- 函数

  ```ts
  function warnUser(): void {
      console.log("This is my warning message");
  }
  ```

- 变量

  > 声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`

  ```ts
  let unusable: void = undefined;
  ```

### Null 和 Undefined

> 默认情况下`null`和`undefined`是所有类型的子类型。 
>
> 可以把 `null`和`undefined`赋值给`任意`类型的变量。

```ts
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

### never 永不存在的值的类型

> `never`类型表示的是那些永不存在的值的类型
>
> 会抛出**异常**或根本就**不会有返回值**的函数表达式或箭头函数表达式的**返回值类型**
>
> - `never`类型是任何类型的子类型，也可以赋值给任何类型
>
> - *没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）
>
> -  `any`也不可以赋值给`never`

- never类型的函数返回值

  ```ts
  // 抛异常 返回never的函数必须存在无法达到的终点
  function error(message: string): never {
      throw new Error(message);
  }
  
  // 报错 推断的返回值类型为never, 
  function fail() {
      return error("Something failed");
  }
  
  // 无限循环 返回never的函数必须存在无法达到的终点
  function infiniteLoop(): never {
      while (true) {
      }
  }
  ```

### Object

> `object`表示非原始类型，除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

```ts
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 类型断言(类型转换)

- 尖括号

  ```ts
  let someValue: any = "this is a string"; // any 类型
  
  let strLength: number = (<string>someValue).length; // string类型
  ```

- as语法

  ```ts
  let someValue: any = "this is a string"; // any 类型
  
  let strLength: number = (someValue as string).length; // stinrg 类型
  ```

  

## 函数

### 函数类型

> 函数的类型只是由参数类型和返回值组成的

### 为函数定义类型

> - 形参类型
> - 函数返回值类型：TypeScript能够根据**返回语句**自动推断出返回值类型，因此我们通常省略它。

```ts
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x + y; };
```

### 书写完整函数类型

> - 函数名：myAdd
> - 形参名和类型：x: number, y: number
> - 函数体：= 后的部分
> - 返回值：number，在函数和返回值类型之前使用( `=>`)符号

```ts
let myAdd: (x: number, y: number) => number =
    function(x: number, y: number): number { return x + y; };
```

### 推断类型

> 在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript编译器会自动识别出类型
>
> 这叫做“按上下文归类”，是类型推论的一种

```ts
// 左边没有类型
let myAdd = function(x: number, y: number): number { return x + y; };

// `x`，`y` ：number
let myAdd: (baseValue: number, increment: number) => number =
    function(x, y) { return x + y; };
```

### 参数

> 传递给函数的参数个数和类型必须与函数期望的参数个数和类型一致。

```ts
function buildName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error
let result2 = buildName("Bob", "Adams", "Sr.");  // error
let result3 = buildName("Bob", "Adams");         // right
```

#### 可选参数

- 作用：动态参数，可传可不传
- 实现：在参数名旁使用 `?`实现可选参数的功能
- 注意：可选参数必须跟在必须参数后面

```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob");  // ok
let result2 = buildName("Bob", "Adams", "Sr.");  // error
let result3 = buildName("Bob", "Adams");
```

#### 默认参数

- 作用：给参数赋默认值

```ts
function buildName(firstName: string, lastName = "Smith") {
    // ...
}
```

#### 剩余参数

- 作用：想同时操作多个参数，或入参个数未知
- 实现：把所有参数收集到数组变量里

```ts
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```



