## 1.验证一个数是否是素数

1、如果这个数是 2 或 3，一定是素数；
2、如果是偶数，一定不是素数；
3、如果这个数不能被3~它的平方根中的任一数整除，m必定是素数。而且除数可以每次递增（排除偶数）

```javascript
function isPrime(num){
    if (num === 2 || num === 3) {
        return true;
    };
    if (num % 2 === 0) {
        return false;
    };
    let divisor = 3,
        limit = Math.sqrt(num);
    while(limit >= divisor){
        if (num % divisor === 0) {
            return false;
        }
        else {
            divisor += 2;
        }
    }
    return true;
}
console.log(isPrime(30));  // false
```

## 2.斐波那契

最简单的做法：递归。

```javascript
function fibonacci(n){
    if (n <= 0) { 
        return 0;
    }
    if (n == 0) {
        return 1;
    }
    return fibonacci(n-1) + fibonacci(n-2);
}
```

但是递归会有严重的效率问题。比如想要求得f(10)，首先需要求f(9)和f(8)。同样，想求f(9)，首先需要f(8)和f(7)…这样就有很多重复值，计算量也很大。

改进：从下往上计算，首先根据f(0)和f(1)计算出f(2)，再根据f(1)和f(2)计算出f(3)……以此类推就可以计算出第n项。时间复杂度O(n)。

```javascript
function fibonacci(n){
    let ori = [0,1];
    if (n < 2) {
        return ori[n];
    };
    let fiboOne = 1,
        fiboTwo = 0,
        fiboSum = 0;
    for (let i = 2; i <= n; i++) {
        fiboSum = fiboOne + fiboTwo;
        fiboTwo = fiboOne;
        fiboOne = fiboSum;
    }
    return fiboSum;
}
console.log(fibonacci(5));
```

## 3、求最大公约数

除数 在a和b的范围内，如果同时a和b处以除数的余等于0，就将此时的除数赋值给res；除数自增，不断循环上面的计算，更新res。

```javascript
function greatestCommonDivisor(a, b){
    let divisor = 2,
        res = 1;
    if (a < 2 || b < 2) {
        return 1;
    };
    while(a >= divisor && b >= divisor){
        if (a%divisor === 0 && b%divisor === 0) {
            res = divisor;
        }
        divisor++;
    }
    return res;
};
console.log(greatestCommonDivisor(8, 4)); 
// 4
console.log(greatestCommonDivisor(69, 169)); 
// 1
```

解法2：

```javascript
function greatestCommonDivisor(a,b){
    if (b === 0) {
        return a;
    } else {
        return greatestCommonDivisor(b,a%b);
    }
};
```

## 4、数组去重

对原数组进行遍历
    获取arr[i]的值 j；
    对应到辅助数组 exits 的位置 j 的值，如果没有，则证明arr[i] 的值没有重复，
  此时将 值j 存入res数组，并将辅助数组 j 位置的值置为 true。
最后返回res数组。

```javascript
function removeDuplicate(arr){
    if (arr === null || arr.length < 2) {
        return arr;
    };
    let res = [],
        exits = [];
    for(let i = 0; i < arr.length; i++){
        let j = arr[i];
        while( !exits[j] ){
            res.push(arr[i]);
            exits[j] = true;
        }
    }
    return res;
}
console.log(removeDuplicate([1,3,3,3,1,5,6,7,8,1]))
// [1,3,5,6,7,8]
```

## 5、删除重复的字符

这一题的解法和上一题类似。

```javascript
function removeDuplicateChar(str){
    if (!str || str.length < 2 || typeof str != "string") {
        return;
    };
    let charArr = [],res = [];
    for(let i = 0; i < str.length; i++){
        let c = str[i];
        if(charArr[c]){ 
            charArr[c]++;
        }
        else{
            charArr[c] = 1;
        }
    }
    for(let j in charArr){
        if (charArr[j] === 1) {
            res.push(j);
        }
    }
    return res.join("");
}
console.log(removeDuplicateChar("Learn more javascript dude"));
// Lnmojvsciptu
```

## 6、排序两个已经排好序的数组

如果 b数组已经遍历完，a数组还有值 或 a[i] 的值 小于等于 b[i] 的值，则将 a[i] 添加进数组res，并 i++；
如果不是上面的情况，则将 b[i] 添加进数组res，并 i++；

```javascript
function mergeSortedArr(a,b){
    if (!a || !b) {
        return;
    };
    let aEle = a[0],bEle = b[0],i = 1,j = 1,res = [];
    while(aEle || bEle){
        if ((aEle && !bEle) || aEle <= bEle) {
            res.push(aEle);
            aEle = a[i++];
        }
        else{
            res.push(bEle);
            bEle = b[j++];
        }
    }
    return res;
}
console.log(mergeSortedArr([2,5,6,9], [1,2,3,29]))
// [1,2,2,3,5,6,9,29]
```

## 7、字符串反向

```javascript
function reverse(str){
    let resStr = "";
    for(let i = str.length-1; i >= 0; i--){ 
        resStr += str[i];
    }
    return resStr;
}
console.log(reverse("ABCDEFG"));
function reverse2(str){
    if (!str || str.length < 2 || typeof str != "string") {
        return str;
    }; 
    let res = [];
    for(let i = str.length-1; i >= 0; i--){
        res.push(str[i]);
    }
    return res.join("");
}
console.log(reverse2("Hello"));
```

将函数添加到String.prototype

```javascript
String.prototype.reverse3 = function(){
    if (!this || this.length < 2) {
        return;
    };
    let res = [];
    for(let i = this.length-1; i >= 0; i--){
        res.push(this[i]);
    } 
    return res.join("");
}
console.log("abcdefg".reverse3());
```

## 8、字符串原位反转

例如：将“I am the good boy”反转变为 “I ma eht doog yob”。

提示：使用数组和字符串方法。

```javascript
function reverseInPlace(str){
    return str.split(' ').reverse().join(' ').split('').reverse().join('');
}
console.log(reverseInPlace('I am the good boy'));
```

## 9.判断是否是回文

```javascript
function isPalindrome(str){
    if (!str || str.length < 2) {
        return;
    }
    for(let i = 0; i < str.length/2; i++){
        if (str[i] !== str[str.length-1-i]) {
            return false;
        }
    }
    return true;
}
console.log(isPalindrome("madama"))
```

## 10.判断数组中是否有两数之和

eg：在一个未排序的数组中找出是否有任意两数之和等于给定的数。

给出一个数组[6,4,3,2,1,7]和一个数9，判断数组里是否有任意两数之和为9。

这个题解得很巧妙，

1、循环遍历数组，let subStract = num - arr[i];
2、如果 differ[subStract] 里有值，则返回true；如果没有，将 differ[arr[i]] 置为 true。

```javascript
function sumFind(arr,num){
    if (!arr || arr.length < 2) {
        return;
    };
    let differ = {};
    for(let i = 0; i < arr.length; i++){
        let subStract = num - arr[i];
        if (differ[subStract]) {
            return true;
        }
        else{
            differ[arr[i]] = true;
        }
    }
    return false;
}
console.log(sumFind([6,4,3,2,1,7], 9));
// true
```

## 11.连字符转成驼峰

如：get-element-by-id 转为 getElementById

```javascript
let str = 'get-element-by-id';
let arr = str.split('-');
for(let i=1; i<arr.length; i++){
    arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].substring(1);
}
console.log(arr.join(''));
// getElementById
```

## 12.加油站问题-贪心算法

一辆汽车加满油后可行驶n公里。旅途中有若干个加油站。设计一个有效算法，指出应在哪些加油站停靠加油，使沿途加油次数最少。对于给定的n(n <= 5000)和k(k <= 1000)个加油站位置，编程计算最少加油次数。并证明算法能产生一个最优解。
要求：

输入：第一行有2个正整数n和k，表示汽车加满油后可行驶n公里，且旅途中有k个加油站。接下来的1 行中，有k+1 个整数，表示第k个加油站与第k-1 个加油站之间的距离。第0 个加油站表示出发地，汽车已加满油。第k+1 个加油站表示目的地。

输出：输出编程计算出的最少加油次数。如果无法到达目的地，则输出”NoSolution”。

```javascript
function greedy(n, k, arr){
    // n:加满可以行驶的公里数; k:加油站数量; arr:每个加油站之间的距离数组
    if (n == 0 || k == 0 || arr.length == 0 || arr[0] > n) {
        return "No Solution!";
        // arr[0] > n ：如果第一个加油站距离太远，也无法到达
    };
    let res = 0, distance = 0;
    // res：加油次数；distance：已行驶距离
    for(let i = 0; i <= k; i++){
        distance += arr[i];
        if (distance > n) {
            // 已行驶距离 > 加满可以行驶的公里数
            if(arr[i] > n){
                // 如果目前加油站和前一个加油站的距离 > 加满可以行驶的公里数，则无法到达 
                return "No Solution!";
            };
            // 可以在上一个加油站加油，行驶到目前的加油站i：
            distance = arr[i];
            res++;  // 加油次数+1 
        }
    }
    return res;
}
let arr = [1,2,3,4,5,1,6,6];
console.log(greedy(7,7,arr))
// 4
```

## 13.用正则实现trim() 清除字符串两端空格

```javascript
String.prototype.trim1 = function(){ 
    // return this.replace(/\s*/g,"");
    // 清除所有空格
    return this.replace(/(^\s*)|(\s*$)/g,"");
    // 清除字符串前后的空格
};
console.log("  hello word ".trim1()) 
// "hello word"
```

## 14.将数字12345678转化成RMB形式：12,345,678

思路：将字符串切割成数组再反转，遍历数组，加入辅助数组，当数组长度为3的倍数，再向辅助数组加入 ","。

```javascript
function RMB(str){
    let arr = str.split("").reverse();
    let res = [];
    for(let i = 0; i < arr.length; i++){
        res.push(arr[i]);
        if ((i + 1) % 3 === 0) {
            res.push(",");
        }
    }
    return res.reverse().join("");
}
console.log(RMB("12345678"))
```

## 15.删除相邻相同的字符串

```javascript
function delSrt(str){
    let res = [],
        nowStr;
    for(let i = 0; i < str.length; i ++){
        if (str.charAt(i) != nowStr) {
            res.push(str.charAt(i));
            nowStr = str.charAt(i);
        }
    }
    return res.join("");
}
console.log(delSrt("aabcc11"))
```

## 16.渲染十万条数据

```javascript
//需要插入的容器
      let ul = document.getElementById('container');
      // 插入十万条数据
      let total = 100000;
      // 一次插入 20 条
      let once = 20;
      //总页数
      let page = total / once
      //每条记录的索引
      let index = 0;
      //循环加载数据
      function loop(curTotal, curIndex) {
        if (curTotal <= 0) {
          return false;
        }
        //每页多少条
        let pageCount = Math.min(curTotal, once);
        window.requestAnimationFrame(function () {
          let fragment = document.createDocumentFragment();
          for (let i = 0; i < pageCount; i++) {
            let li = document.createElement('li');
            li.innerText = curIndex + i + ' : ' + ~~(Math.random() * total)
            fragment.appendChild(li)
          }
          ul.appendChild(fragment)
          loop(curTotal - pageCount, curIndex + pageCount)
        })
      }
      loop(total, index);
```

## 17.deepclone深拷贝

``` javascript
const MY_IMMER = Symbol('my-immer1')

const isPlainObject = value => {
  if (
    !value ||
    typeof value !== 'object' ||
    {}.toString.call(value) != '[object Object]'
  ) {
    return false
  }
  var proto = Object.getPrototypeOf(value)
  if (proto === null) {
    return true
  }
  var Ctor = hasOwnProperty.call(proto, 'constructor') && proto.constructor
  return (
    typeof Ctor == 'function' &&
    Ctor instanceof Ctor &&
    Function.prototype.toString.call(Ctor) ===
      Function.prototype.toString.call(Object)
  )
}

const isProxy = value => !!value && !!value[MY_IMMER]

function produce(baseState, fn) {
  const proxies = new Map()
  const copies = new Map()

  const objectTraps = {
    get(target, key) {
      if (key === MY_IMMER) return target
      const data = copies.get(target) || target
      return getProxy(data[key])
    },
    set(target, key, val) {
      const copy = getCopy(target)
      const newValue = getProxy(val)
      // 这里的判断用于拿 proxy 的 target
      // 否则直接 copy[key] = newValue 的话外部拿到的对象是个 proxy
      copy[key] = isProxy(newValue) ? newValue[MY_IMMER] : newValue
      return true
    }
  }

  const getProxy = data => {
    if (isProxy(data)) {
      return data
    }
    if (isPlainObject(data) || Array.isArray(data)) {
      if (proxies.has(data)) {
        return proxies.get(data)
      }
      const proxy = new Proxy(data, objectTraps)
      proxies.set(data, proxy)
      return proxy
    }
    return data
  }

  const getCopy = data => {
    if (copies.has(data)) {
      return copies.get(data)
    }
    const copy = Array.isArray(data) ? data.slice() : { ...data }
    copies.set(data, copy)
    return copy
  }

  const isChange = data => {
    if (proxies.has(data) || copies.has(data)) return true
  }

  const finalize = data => {
    if (isPlainObject(data) || Array.isArray(data)) {
      if (!isChange(data)) {
        return data
      }
      const copy = getCopy(data)
      Object.keys(copy).forEach(key => {
        copy[key] = finalize(copy[key])
      })
      return copy
    }
    return data
  }

  const proxy = getProxy(baseState)
  fn(proxy)
  return finalize(baseState)
}

const state = {
  info: {
    name: 'yck',
    career: {
      first: {
        name: '111'
      }
    }
  },
  data: [1]
}

const data = produce(state, draftState => {
  draftState.info.age = 26
  draftState.info.career.first.name = '222'
})

console.log(data, state)
console.log(data.data === state.data)
```

[原文地址](https://mp.weixin.qq.com/s/M7KBX3w2KqlWhZFHJSYP6Q)

## 1..过滤唯一值

`Set`对象类型是在ES6中引入的，配合展开操作`...`一起，我们可以使用它来创建一个新数组，该数组只有唯一的值。

```javascript
const array = [1, 1, 2, 3, 5, 5, 1]
const uniqueArray = [...new Set(array)];
console.log(uniqueArray); // Result: [1, 2, 3, 5]
```

在ES6之前，隔离惟一值将涉及比这多得多的代码。

此技巧适用于包含基本类型的数组：`undefined`，`null`，`boolean`，`string`和`number`。（如果你有一个包含对象，函数或其他数组的数组，你需要一个不同的方法！）

\##2. 与或运算

三元运算符是编写简单（有时不那么简单）条件语句的快速方法，如下所示：

```javascript
x > 100 ? 'Above 100' : 'Below 100';
x > 100 ? (x > 200 ? 'Above 200' : 'Between 100-200') : 'Below 100';
```

但有时使用三元运算符处理也会很复杂。相反，我们可以使用'与'`&&`和'或'`||` 逻辑运算符以更简洁的方式书写表达式。这通常被称为“短路”或“短路运算”。

#### **它是怎么工作的**

假设我们只想返回两个或多个选项中的一个。

使用`&&`将返回第一个条件为`假`的值。如果每个操作数的计算值都为`true`，则返回最后一个计算过的表达式。

```javascript
let one = 1,
    two = 2,
    three = 3;
console.log(one && two && three); // Result: 3
console.log(0 && null); // Result: 0
```

使用`||`将返回第一个条件为`真`的值。如果每个操作数的计算结果都为`false`，则返回最后一个计算过的表达式。

```javascript
let one = 1,
    two = 2,
    three = 3;
console.log(one || two || three); // Result: 1
console.log(0 || null); // Result: null
```

#### **例一**

假设我们想返回一个变量的长度，但是我们不知道变量的类型。

我们可以使用`if/else`语句来检查`foo`是可接受的类型，但是这可能会变得非常冗长。或运行可以帮助我们简化操作：

```javascript
return (foo || []).length
```

如果变量`foo`是true，它将被返回。否则，将返回空数组的长度:`0`。

#### **例二**

你是否遇到过访问嵌套对象属性的问题？你可能不知道对象或其中一个子属性是否存在，这可能会导致令人沮丧的错误。

假设我们想在`this.state`中访问一个名为`data`的属性，但是在我们的程序成功返回一个获取请求之前，`data` 是未定义的。

根据我们使用它的位置，调用`this.state.data`可能会阻止我们的应用程序运行。为了解决这个问题，我们可以将其做进一步的判断：

```javascript
if (this.state.data) {  return this.state.data;} else {  return 'Fetching Data';}
```

但这似乎很重复。'`或'` 运算符提供了更简洁的解决方案：

```javascript
return (this.state.data || 'Fetching Data');
```

#### **一个新特性: Optional Chaining**

过去在 Object 属性链的调用中，很容易因为某个属性不存在而导致之后出现`Cannot read property xxx of undefined`的错误。

那 `optional chaining` 就是添加了`?.`这么个操作符，它会先判断前面的值，如果是 `null`或 `undefined`，就结束调用、返回 `undefined`。

例如，我们可以将上面的示例重构为 `this.state.data?.()`。或者，如果我们主要关注`state` 是否已定义，我们可以返回`this.state？.data`。

该提案目前处于第1阶段，作为一项实验性功能。你可以在这里阅读它，你现在可以通过Babel使用你的JavaScript，将 @babel/plugin-proposal-optional-chaining添加到你的`.babelrc`文件中。

## 3.转换为布尔值

除了常规的布尔值`true`和`false`之外，JavaScript还将所有其他值视为 **‘truthy’** 或**‘falsy’**。

除非另有定义，否则 JavaScript 中的所有值都是'truthy'，除了 `0`，`“”`，`null`，`undefined`，`NaN`，当然还有`false`，这些都是**'falsy'**

我们可以通过使用负算运算符轻松地在`true`和`false`之间切换。它也会将类型转换为“boolean”。

```javascript
const isTrue  = !0;
const isFalse = !1;
const alsoFalse = !!0;
console.log(isTrue); // Result: true
console.log(typeof true); // Result: "boolean"
```

## 4. 转换为字符串

要快速地将数字转换为字符串，我们可以使用连接运算符`+`后跟一组空引号`""`。

```javascript
const val = 1 + "";console.log(val); // Result: "1"
console.log(typeof val); // Result: "string"
```

## 5. 转换为数字

使用加法运算符`+`可以快速实现相反的效果。

```javascript
let int = "15";
int = +int;console.log(int); // Result: 15
console.log(typeof int); //Result: "number"
```

这也可以用于将布尔值转换为数字，如下所示

```javascript
 console.log(+true);  // Return: 1 
console.log(+false); // Return: 0
```

在某些上下文中，`+`将被解释为连接操作符，而不是加法操作符。当这种情况发生时(你希望返回一个整数，而不是浮点数)，您可以使用两个波浪号:`~~`。

连续使用两个波浪有效地否定了操作，因为`— ( — n — 1) — 1 = n + 1 — 1 = n`。换句话说，`~—16` 等于`15。`

```javascript
const int = ~~"15"
console.log(int); // Result: 15
onsole.log(typeof int);// Result: "number"
```

虽然我想不出很多用例，但是按位NOT运算符也可以用在布尔值上：`~true = -2`和`~false = -1`。

## 6.性能更好的运算

从ES7开始，可以使用指数运算符`**`作为幂的简写，这比编写`Math.pow(2, 3)` 更快。这是很简单的东西，但它之所以出现在列表中，是因为没有多少教程更新过这个操作符。

```javascript
console.log(2 ** 3); // Result: 8
```

这不应该与通常用于表示指数的^符号相混淆，但在JavaScript中它是按位`异或`运算符。

在ES7之前，只有以`2`为基数的幂才存在简写，使用按位左移操作符`<<`

```javascript
Math.pow(2, n);2 << (n - 1);2**n;
```

例如，`2 << 3 = 16`等于`2 ** 4 = 16`。

## 7. 快速浮点数转整数

如果希望将浮点数转换为整数，可以使用`Math.floor()`、`Math.ceil()`或`Math.round()`。但是还有一种更快的方法可以使用`|`(位或运算符)将浮点数截断为整数。

```javascript
console.log(23.9 | 0);  // Result: 23
console.log(-23.9 | 0); // Result: -23
```

`|`的行为取决于处理的是正数还是负数，所以最好只在确定的情况下使用这个快捷方式。

如果`n`为正，则`n | 0`有效地向下舍入。如果`n`为负数，则有效地向上舍入。更准确地说，此操作将删除小数点后面的任何内容，将浮点数截断为整数。

你可以使用`~~`来获得相同的舍入效果，如上所述，实际上任何位操作符都会强制浮点数为整数。这些特殊操作之所以有效，是因为一旦强制为整数，值就保持不变。

#### **删除最后一个数字**

`按位或`运算符还可以用于从整数的末尾删除任意数量的数字。这意味着我们不需要使用这样的代码来在类型之间进行转换。

```javascript
let str = "1553";
Number(str.substring(0, str.length - 1));
```

相反，按位或运算符可以这样写：

```javascript
console.log(1553 / 10   | 0)  // Result: 155
console.log(1553 / 100  | 0)  // Result: 15
console.log(1553 / 1000 | 0)  // Result: 1
```

## 8. 类中的自动绑定

我们可以在类方法中使用ES6箭头表示法，并且通过这样做可以隐含绑定。这通常会在我们的类构造函数中保存几行代码，我们可以愉快地告别重复的表达式，例如`this.myMethod = this.myMethod.bind(this)`

```javascript
import React, { Component } from React;
export default class App extends Compononent {
    constructor(props) {
        super(props);
        this.state = {};
    }
    myMethod = () => {
        // This method is bound implicitly!
    }
    render() {
        return (
            <> 
            <div>
            {this.myMethod()
    }
    </div>
    </>
)
}};
```

## 9. 数组截断

如果要从数组的末尾删除值，有比使用`splice()`更快的方法。

例如，如果你知道原始数组的大小，您可以重新定义它的`length`属性，就像这样

```javascript
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
array.length = 4;console.log(array); // Result: [0, 1, 2, 3]
```

这是一个特别简洁的解决方案。但是，我发现`slice()`方法的运行时更快。如果速度是你的主要目标，考虑使用：

```javascript
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
array = array.slice(0, 4);
console.log(array); // Result: [0, 1, 2, 3]
```

## 10. 获取数组中的最后一项

数组方法`slice()`可以接受负整数，如果提供它，它将接受数组末尾的值，而不是数组开头的值。

```javascript
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log(array.slice(-1)); // Result: [9]
console.log(array.slice(-2)); // Result: [8, 9]
console.log(array.slice(-3)); // Result: [7, 8, 9]
```

## 11.格式化JSON代码

最后，你之前可能已经使用过`JSON.stringify`，但是您是否意识到它还可以帮助你缩进JSON？

`stringify()`方法有两个可选参数：一个`replacer`函数，可用于过滤显示的JSON和一个空格值。

```javascript
console.log(JSON.stringify({ alpha: 'A', beta: 'B' }, null, '\t'));
// Result:
// '{
//     "alpha": A,
//     "beta": B
// }'
```