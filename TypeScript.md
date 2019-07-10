## 数据类型
### String类型
一个保存字符串的文本，类型声明为 string。可以发现类型声明可大写也可小写，后文同理。
```typescript
let name1: string = 'chris'
let name2: String = 'chris'
```
### Boolean类型
boolean是 true 或 false 的值，所以 let isBool3: boolean = new Boolean(1) 就会编译报错，因为 new Boolean(1) 生成的是一个 Bool 对象。
```typescript
let isBool: boolean = false
```
### Number类型
```typescript
let num: number = 3
```
### Array类型
```typescript
let arr1: number[] = [1, 2, 3]
let arr2: Array<number> = [1, 2, 3]

let arr3: string[] = ['chris', 'james', 'eric']
let arr4: Array<string> = ['chris', 'james', 'eric']
```
### Enums类型
```typescript
enum Role {Employee = 3, Manager, Admin}
let role: Role = Role.Employee
console.log(role) 
```
### Any类型
```typescript
let notSure1: any = 18
let notSure2: any[] = [3, 'chris', true]
```
### Void类型
```typescript
function alertName(): void {
    console.log('chris')
}
```

## 函数
### 为函数定义类型
我们可以给每个参数添加类型之后再为函数本身添加返回值类型。 TypeScript能够根据返回语句自动推断出返回值类型，因此我们通常省略它。下面函数 add1, add2, add3 的效果是一样的，其中是 add3 函数是函数完整类型。
```typescript
function add1(x: string, y: string): string {
    return 'hello ts'
}

let add2 = function add2(x: string, y: string): string {
    return 'hello ts'
}

let add3: (x: string, y: string) => string = function(x: string, y: string): string {
    return 'hello ts'
}
```
### 可选参数和默认参数
JavaScript 里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是 undefined 。 在 TypeScript 里我们可以在参数名旁使用?实现可选参数的功能。 比如，我们想让 lastname 是可选的：
```typescript
function buildName(firstName: srting, lastName?: string) {
    console.log(lastName ? firstName + '' + lastName : firstName)
}

let res1 = buildName('chris', 'paul')
let res2 = buildName('tracy')
let res3 = buildName('michel', 'carter', 'williams') // 编译失败
```
如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined 值来获得默认值。 例如，我们重写上例子，让 firstName 是带默认值的参数：
```typescript
function checkName(firstName = 'chris', lastName?: string){
    console.log(firstName + '' + lastName)
}

let res1 = checkName('paul') //paulundefined
let res2 = checkName(undefined, 'paul') //chrispaul
```
## 类
### 类
```typescript
class Person {
    name: string
    age: number
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
    print() {
        return this.name + this.age
    }
}

let person: Person = new Person('chris', 34)
console.log(person.print()) //chris34
```
我们在引用任何一个类成员的时候都用了 this。 它表示我们访问的是类的成员。其实这本质上还是 ES6 的知识，只是在 ES6 的基础上多上了对 this 字段和引用参数的类型声明。
### 继承
```typescript
class Person {
    public name: string
    age: number
    constructor(name: string, age: number) {
        this.name = name 
        this.age = age 
    }
    tell() {
        console.log(this.name + this.age)
    }
}

class Player extends Person {
    gender: string
    constructor(gender: string) {
        super('chris', 34)
        this.gender = gender
    }
    tell() {
        console.log(this.name + this.age + this.gender)
    }
}

let info = new Player('male')
info.tell() //chris34male
```
这个例子展示了 TypeScript 中继承的一些特征，可以看到其实也是 ES6 的知识上加上类型声明。不过这里多了一个知识点 —— 公共，私有，以及受保护的修饰符。TypeScript 里，成员默认为 public ；当成员被标记成 private 时，它就不能在声明它的类的外部访问；protected 修饰符与private 修饰符的行为很相似，但有一点不同，protected 成员在派生类中仍然可以访问。
### 储存器
TypeScript 支持通过 getters/setters 来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。
对于存取器有下面几点需要注意的：首先，存取器要求你将编译器设置为输出 ECMAScript 5 或更高。 不支持降级到 ECMAScript 3。 其次，只带有 get 不带有 set 的存取器自动被推断为 readonly。 这在从代码生成 .d.ts 文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。
```typescript
class Hello {
    private _name: string
    private _age: number
    get name(): string {
        return this._name
    }
    set name(name: string) {
        this._name = name
    }
    get age(): number {
        return this._age
    }
    set age(age: number) {
        if(age > 0 && age < 100) {
            console.log('he is between 1-100')
        }
        this._age = age
    }
}

let hello = new Hello()
hello.name = 'chris'
hello.age = 34 // he is between 1-100
console.log(hello.name) //chris
console.log(hello.age) //34
```
## 接口
### 接口
TypeScript的核心原则之一是对值所具有的结构进行类型检查。在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。
```typescript
interface LabelValue{
    label: string
}

function printLabel(labelObj: LabelValue) {
    console.log(labelObj.label)
}

let myObj = {
    'label': 'hello interface'
}
printLabel(myObj)
```
LabelledValue 接口就好比一个名字，它代表了有一个 label 属性且类型为 string 的对象。只要传入的对象满足上述必要条件，那么它就是被允许的。

另外，类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以。
### 可选属性
带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个 ? 符号。可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。
```typescript
interface Person {
    name?: string
    age?: number
}

function printInfo(info: Person) {
    console.log(info)
}

let info1 = {
    'name': 'chris',
    'age': 34
}
printInfo(info1) //{ name: 'chris', age: 34 }

let info2 = {
    'name': 'james'
}
printInfo(info2) //{ name: 'james' }
```
### 函数类型
接口能够描述 JavaScript 中对象拥有的各种各样的外形。 除了描述带有属性的普通对象外，接口也可以描述函数类型。定义的函数类型接口就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。定义后完成后，我们可以像使用其它接口一样使用这个函数类型的接口。
```typescript
interface SearchFunc {
    (source: string, subString: string): boolean
}

let mySearch: SearchFunc
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1
}

console.log(mySearch('chrispaul', 'chris')) //true
console.log(mySearch('james', 'harden')) //false
```
### 可引索类型
与使用接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如 a[10] 或 ageMap["daniel"]。 可索引类型具有一个索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。 让我们看如下例子：
```typescript
interface StringArray {
    [index: number]: string
}

let myArray: StringArray
myArray = ['chris', 'james', 'eric']
console.log(myArray[0])
```
### 类类型？
与 C# 或 Java 里接口的基本作用一样，TypeScript 也能够用它来明确的强制一个类去符合某种契约。

我们可以在接口中描述一个方法，在类里实现它，如同下面的 setTime 方法一样：
```typescript
interface ClockInterface{
  currentTime: Date;
  setTime(d: Date);
}

class Clock implements ClockInterface{
  currentTime: Date;
  setTime(d: Date){
      this.currentTime = d;
  }
  constructor(h: number, m: number) {}
}
```
### 继承接口
和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。
```typescript
interface Color {
    color: string
}

interface Shape {
    shape: string
}

interface Appearance extends Color, Shape {
    id: number
}

let myApp = <Appearance>{}
myApp.color = 'blue'
myApp.shape = 'sphere'
myApp.id = 3
console.log(myApp)
```
## 模块
TypeScript 与 ECMAScript 2015 一样，任何包含顶级 import 或者 export 的文件都被当成一个模块。
```typescript
export interface StringValidator{
    isAcceptable(s:string): boolean;
}

var strReg = /^[A-Za-z]+$/;
var numReg = /^[0-9]+$/;

export class letterValidator implements StringValidator{
    isAcceptable(s:string): boolean{
        return strReg.test(s);
    }
}

export class zipCode implements StringValidator{
    isAcceptable(s: string): boolean{
        return s.length == 5 && numReg.test(s);
    }
}

let a = new letterValidator()
console.log(a.isAcceptable('chris')) //true
let b = new zipCode()
console.log(b.isAcceptable('12345')) //true
```
## 泛型
### 初探泛型
软件工程中，我们不仅要创建一致的定义良好的 API ，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。在像 C# 和 Java 这样的语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

要兼容多种数据格式，可能会有人想到any。
```typescript
function indentify(arg: any): any {
    return arg
}
```
使用any存在一个问题，有可能传入的值和返回的值不是同一种值，例如，传入数字，但是不确定返回的是什么值。

要解决这个问题，我们需要引入类型变量--一种特殊的变量，只用于表示类型不表示值。
```typescript
function identify<T>(arg: T): T {
    return arg
}

let outPut1 = identify<string>('Hello Generic')
let outPut2 = identify('Hello Generic')
console.log(outPut1)
console.log(outPut2)
```
给identify添加了类型变量T，用来捕获传入值的类型，然后将返回值的类型也设置为T,就实现了传入值和返回值为同一类型值的需求。

我们把identify这个函数叫做泛型，因为它适用于所有类型，并且不会有any类型存在的问题。代码中 outPut1 和 outPut2 是效果是相同的，第二种方法更加普遍，利用了类型推论 —— 即编译器会根据传入的参数自动地帮助我们确定T的类型。
### 泛型变量
在泛型中，我们要合理正确的使用泛型变量T，要牢记T表示任何类型。
```typescript
function identify<T>(arg: T): T {
    console.log(arg.length) //Property 'length' does not exist on type 'T'
    return arg
}
```
在泛型中我们使用了length这个属性，但是T代表任何类型，所以有可能是number，而number是没有length属性的，所以会报错。

如果想要使用length这个属性，我们可以创建数组。
```typescript
function identify<T>(arg: T[]): T[] {
    console.log(arg.length)
    return arg
}
```
### 泛型函数
泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面，像函数声明一样。
```typescript
function identify<T>(arg: T): T {
    return arg
}

let myIdentify: <U>(arg: U) => U = identify
```
从上面的代码中可以看出也可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。

当然也可以把泛型参数当做一个接口的参数，这样就可以知道这个接口具体用的是那种类型。
```typescript
interface GenericIdentify<T> {
    (arg: T): T
}

function identify<T>(arg: T): T {
    return arg
}

let myGenericidentify: GenericIdentify<string> = identify
```
### 泛型类？
泛型类看上去与泛型接口差不多。 泛型类使用（<>）括起泛型类型，跟在类名后面。
```typescript
class GenericNumber<T, U> {
  myValue: T
  myName<U>(x: U): U {
    return x
  }
  constructor(value: T) {
    this.myValue = value
  }
}

let myGenericNumber = new GenericNumber<number, number>(0)
myGenericNumber.myValue = 3
console.log(myGenericNumber.myValue) //3
console.log(myGenericNumber.myName('chris')) //chris
```
### 泛型约束
在前面的泛型变量中遇到了一个问题，就是在泛型中调用参数的length时，如果参数没有Length属性会报错，而使用泛型约束，就是只有满足一定的条件才可以使用这个泛型

为此，我们定义一个接口来描述约束条件。 创建一个包含 .length属性的接口，使用这个接口和extends关键字还实现约束：
```typescript
interface lengthwise{
    length: number
}

function identity<T extends lengthwise>(arg: T): T{
    console.log(arg.length)
    return arg
}

identity(3) //Argument of type '3' is not assignable to parameter of type 'lengthwise'
identity('chris') //5
```


