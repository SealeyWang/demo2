
前端基础知识
1. 介绍⼀下CSS盒模型

盒模型 从外到内分别是 margin border padding content

content-box width 指的是 content的宽度

border-box  width 指的是 border padding content 

2. display: none 和 visibility:hidden 的区别

两者元素都还html里， 但是

display: node 的元素没有宽高

visibility: hidden 元素只是看不见，但是元素仍在文档流中占位，且可以操作，如点击

3. 写出以下代码的输出
```JavaScript
console.log(1);
setTimeout(function () { console.log(2); }, 0);
new Promise(resolve => {
    console.log(3);
    resolve();
    console.log(4);
}).then(() => {
    console.log(5);
})
console.log(6);
```
1 3 4 6 5 2

1 3 4 6 是普通任务  
5 在then中执行的是微任务 在eventLoop某个阶段结束后立刻执行， resolve()在普通任务阶段调用 所以普通任务完成后打印5

2 是宏任务，在timers阶段执行， 当普通任务执行完后 执行timers阶段的代码

4. 写出以下代码的输出
```JavaScript
var name = 1
function test() {
    var name = 2
    console.log(this.name)
    return function inner() {
        console.log(name)
    }
}
test()  //1      
test()() // 1 2
var b = {
    name: 3
}
b.test = test
b.test() // 3 
var c = b.test
c() // 1
new test() //undefined  
```
1 1 2 3 1 2 

test() =>test.call(undefined) //  当前为非严格模式， this 为window 1
test()()   先打印1  闭包 打印2 

b.test() => b.call(b) //this = b    3
c() => c.call(undefined)  当前为非严格模式， this 为window  this.name = 1 

new test() new关键创建对象， 使test 变为构造函数，使用严格模式 this为undefined    打印this.name = undefined 


5. 实现⼀个栈数据结构，接⼝如下，请实现该类的 in、out、top、size 函数
```TypeScript
class Stack {
    constructor() {
        this.arr = []
    }
    in(value) {
// 你的代码
        this.arr.push(value)
    }
    out() {
// 你的代码
      return this.arr.pop()
    }
    top() {
// 你的代码
        return this.arr[this.arr.length - 1]
    }
    size() {
// 你的代码
        return this.arr.length
    }
}
// 要求当执⾏下列代码时，能输出预期的结果
const stack = new Stack()
stack.in('x')
stack.in('y')
stack.in('z')
stack.top() // 输出 'z'
stack.size() // 输出 3
stack.out() // 输出 'z'
stack.top() // 输出 'y'
stack.size() // 输出 2
```

编程题
6. 任选框架（React、Vue、Angular）实现⼀个Message组件，满⾜弹出消息通知、关闭消息通知
即可
代码地址： https://codesandbox.io/s/message-hrw3um?file=/src/Message.js
```js
import React from "react";
const Message = (
    config = {
        visibility: false,
        msg: ""
    }
) => {
    const display = config.visibility ? "block" : "none";
    const style = {
        display,
        position: "fixed",
        top: "50px",
        left: "50%",
        transform: "translateX(-50%)",
        border: "1px solid red"
    };
    return <div style={style}>{config.msg}</div>;
};

export default Message;

```

```js
import { useState } from "react";
import Message from "./Message";
import "./styles.css";

export default function App() {
  let [visibility, setVisibility] = useState(true);
  return (
    <div className="App">
      <Message visibility={visibility} msg="123" />
      <button onClick={() => setVisibility(!visibility)}>触发</button>
    </div>
  );
}

```

7. 实现⼀个能对数组去重的⽅法
```JavaScript
// 输⼊ array = [1,5,2,3,2,5,4] 输出 [1, 5, 2, 3, 4]
function unique(array) {
 const map = new Map()
    for(let i=0; i< array.length;i++) {
        let number = array[i]
        if(number === undefined) continue
        if(map.has(number)) continue 
        map.set(number, true)
    }
    return [...map.keys()]
}
```

8. ⽤Promise对fetchData进⾏包装，将回调的设计封装成then的形式
```JavaScript
function fetchData(callback) {
    setTimeout(() => {
        callback("我是返回的数据")
    }, 5000)
}
// 实现下⾯的函数
function promiseFecth() {
    return new Promise((resolve, reject) => {
        fetchData((data)=> {
            resolve(data)
        })
    })
}
promiseFecth().then((res) => {
    console.log(res) // 我是返回的数据
})
```

9. 给出如下地址数据格式，实现函数 getNameById ，输⼊address 和 id ，输出 id 对应的地址
name

```TypeScript
const address = [
    {
        id: 1,
        name: '北京市',
        children: [
            {
                id: 11,
                name: '海淀区',
                children: [
                    {
                        id: 111,
                        name: '中关村',
                    }
                ]
            },
            {
                id: 12,
                name: '朝阳区',
            },
        ],
    },
    {
        id: 2,
        name: '河北省'
    }
]
// 请实现该函数
// 输⼊：getNameById(address, 2)，输出："河北省"
// 输⼊：getNameById(address, 111)，输出："中关村"
// 输⼊：getNameById(address, 32)，输出："" （未查找到，输出空字符串）
function getNameById(address, id) {

    let result = '';
// 在这⾥实现你的代码
    for(let i = 0; i<address.length; i++) {
        const a = address[i];
        if(id === a.id) {
            result = a.name
            break
        }else {
            if(a.children instanceof Array && a.children.length !== 0 ) {
                result =  getNameById(a.children, id)
            }
        }
    }
    return result
}

```

10. ⼩ A 实现了⼀个合并两个数组的函数，假如你来 review 他实现的代码，请从代码实现、代码⻛
格、代码规范等⻆度给出你对下列代码的意⻅

```JavaScript
// ⼩A created at 2021-06-01
let mergeTwoarr=(a1,a2)=>{
    a2.map(item=> {
        a1.push(item)
    })
    console.log('结果是',a1)
    return a1
}
```

首先函数，改变了a1的值，不推荐在函数中改变外部变量的值 ， 而且是代码不够简洁，其次不该把调试log 
也提交了，最后eslint 和 代码格式 可能没有设置

数组合并有 concat  和 扩展运算符 ...  推荐这样做
```js
let mergeTwoarr = (a1,a2) => a1.concat(a2);
let mergeTwoarr = (a1,a2) => [...a1, ...a2];
```

