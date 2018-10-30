# [Getting Started by Tania Rascia](https://www.taniarascia.com/getting-started-with-react/)

## HINT

### JSX

JSX is actually closer to JavaScript, not HTML, so there are a few key differences to note when writing it.

- `className` is used instead of `class` for adding CSS classes, as `class` is a reserved keyword in JavaScript.
- Properties and methods in JSX are camelCase – `onclick` will become `onClick`.
- Self-closing tags *must* end in a slash – e.g. `<img />`

### [ES6基础](https://www.jianshu.com/p/287e0bb867ae)

1. **箭头函数**

```js
//例如ES6：
    [1,2,3].map(x => x + 1)
    
//等同于ES5：
    [1,2,3].map((function(x){
        return x + 1
    }).bind(this))
```

2. **键值对增强**

```JS
// 请使用 ES6 重构一下代码

// 第一题
var jsonParse = require('body-parser').jsonParse

// 第二题
var body = request.body
var username = body.username
var password = body.password


//答案
// 1.
import { jsonParse } from 'body-parser'
// 2. 
const { body, body: { username, password } } = request
```

3. **Spread Operator 展开运算符**

   组装对象或者数组

   ```js
   //数组
   const color = ['red', 'yellow']
   const colorful = [...color, 'green', 'pink']
   console.log(colorful) //[red, yellow, green, pink]
   
   //对象
   const alp = { fist: 'a', second: 'b'}
   const alphabets = { ...alp, third: 'c' }
   console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"}
   
   ```

   有时候我们想获取数组或者对象除了前几项或者除了某几项的其他项

   ```js
   //数组
   const number = [1,2,3,4,5]
   const [first, ...rest] = number
   console.log(rest) //2,3,4,5
   //对象
   const user = {
       username: 'lux',
       gender: 'female',
       age: 19,
       address: 'peking'
   }
   const { username, ...rest } = user
   console.log(rest) //{"address": "peking", "age": 19, "gender": "female"
   }
   ```

   对于 Object 而言，还可以用于组合成新的 Object 。(ES2017 stage-2 proposal) 当然如果有重复的属性名，右边覆盖左边

   ```js
   const first = {
       a: 1,
       b: 2,
       c: 6,
   }
   const second = {
       c: 3,
       d: 4
   }
   const total = { ...first, ...second }
   console.log(total) // { a: 1, b: 2, c: 3, d: 4 }
   ```

4. import导入模块、export导出模块

   ```js
   //全部导入
   import people from './example'
   
   //有一种特殊情况，即允许你将整个模块当作单一对象进行导入
   //该模块的所有导出都会作为对象的属性存在
   import * as example from "./example.js"
   console.log(example.name)
   console.log(example.age)
   console.log(example.getName())
   
   //导入部分
   import {name, age} from './example'
   
   // 导出默认, 有且只有一个默认
   export default App
   
   // 部分导出
   export class App extend Component {};
   
   ```

   以前有人问我，导入的时候有没有大括号的区别是什么。下面是我在工作中的总结：

   ```js
   1.当用export default people导出时，就用 import people 导入（不带大括号）
   
   2.一个文件里，有且只能有一个export default。但可以有多个export。
   
   3.当用export name 时，就用import { name }导入（记得带上大括号）
   
   4.当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 
   
   5.当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example
   ```

5. Promise

   ```
   在promise之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。通过Promise机制，扁平化的代码机构，大大提高了代码可读性；用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。
   ```

   说白了就是用同步的方式去写异步代码。

   发起异步请求

   ```js
       fetch('/api/todos')
         .then(res => res.json())
         .then(data => ({ data }))
         .catch(err => ({ err }));
   ```

   今天看到一篇关于面试题的很有意思。

   ```js
   setTimeout(function() {
       console.log(1)
   }, 0);
   new Promise(function executor(resolve) {
       console.log(2);
       for( var i=0 ; i<10000 ; i++ ) {
           i == 9999 && resolve();
       }
       console.log(3);
   }).then(function() {
       console.log(4);
   });
   console.log(5);
   ```

6. Generators

   ```js
   // 生成器
   function *createIterator() {
       yield 1;
       yield 2;
       yield 3;
   }
   
   // 生成器能像正规函数那样被调用，但会返回一个迭代器
   let iterator = createIterator();
   
   console.log(iterator.next().value); // 1
   console.log(iterator.next().value); // 2
   console.log(iterator.next().value); // 3
   ```


### Component

Simple Component

```jsx
const SimpleComponent = () => { 
    return <div>Example</div>;
}
```

Class Component

```jsx
class ClassComponent extends Component {
    render() {
        return <div>Example</div>;
    }
}
```

Note that if the `return` is contained to one line, it does not need parentheses.



### Props

1. pass data through using `data-` attributes

```js
class App extends Component {
    render() {
        const characters = [
            {
                'name': 'Charlie',
                'job': 'Janitor'
            },
            {
                'name': 'Mac',
                'job': 'Bouncer'
            },
            {
                'name': 'Dee',
                'job': 'Aspring actress'
            },
            {
                'name': 'Dennis',
                'job': 'Bartender'
            }
        ];

        return (
            <div className="container">
                <Table />
            </div>
        );
    }
}
```

The data that’s stored here is known as the **virtual DOM**, which is a fast and efficient way of syncing data with the actual DOM.

2. accessing it 

   This data is not in the actual DOM yet, though. In `Table`, we can access all props through `this.props`. We’re only passing one props through, characterData, so we’ll use `this.props.characterData` to retrieve that data.

   ```js
   class Table extends Component {
       render() {
           const { characterData } = this.props;
   
           return (
               <table>
                   <TableHeader />
                   <TableBody characterData={characterData} />
               </table>
           );
       }
   }
   ```

3. using it 

   ```js
   const TableBody = props => { 
       const rows = props.characterData.map((row, index) => {
           return (
               <tr key={index}>
                   <td>{row.name}</td>
                   <td>{row.job}</td>
               </tr>
           );
       });
   
       return <tbody>{rows}</tbody>;
   }
   ```

   note: mapping through a array

### State

1. create a `state` object

   ```js
   class App extends Component {
       state = {
           characters: [
               {
                   'name': 'Charlie',
                   // the rest of the data
           ]        
        };
   ```

   可临时修改私有数据，不更新到数据库。

2. update data by using `this.setState()`

   ```js
   removeCharacter = index => {
       const { characters } = this.state;
   
       this.setState({
           characters: characters.filter((character, i) => { 
               return i !== index;
           })
       });
   }
   ```

3. passing it

   ```js
   //app.js
   return (
       <div className="container">
           <Table
               characterData={characters}
               removeCharacter={this.removeCharacter} 
           />
       </div>
   );
   ```

4. accessing it 

   ```js
   class Table extends Component {
       render() {
           const { characterData, removeCharacter } = this.props;
   
           return (
               <table>
                   <TableHeader />
                   <TableBody 
                       characterData={characterData} 
                       removeCharacter={removeCharacter} //same as props
                   />
               </table>
           );
       }
   }
   ```

5. using it

   ```js
    <tr key={index}>
       <td>{row.name}</td>
       <td>{row.job}</td>
       <td><button onClick={() => props.removeCharacter(index)}>Delete</button></td>
   </tr>
   ```

### Submitting Form Data

1. create Form component :

   * constructor(props): 初始化表单state

     ```js
     import React, { Component } from 'react';
     
     class Form extends Component {
         constructor(props) {
             super(props);
     
             this.initialState = {
                 name: '',
                 job: ''
             };
     
             this.state = this.initialState;
         }
     }
     ```

   * handleChange = event => {} :  表单的setState操作

     ```js
     handleChange = event => {
         const {name, value} = event.target;
     
         this.setState({
             [name] : value
         });
     }
     ```

   * render() {}: 同步表单state数据

     ```js
     render() {
         const { name, job } = this.state; 
     
         return (
             <form>
                 <label>Name</label>
                 <input 
                     type="text" 
                     name="name" 
                     value={name} 
                     onChange={this.handleChange} />
                 <label>Job</label>
                 <input 
                     type="text" 
                     name="job" 
                     value={job} 
                     onChange={this.handleChange}/>
             </form>
         );
     }
     
     export default Form;
     ```

2. add Form to App:

   * handleSubmit = character => {}  : 定义表单操作

     ```js
     handleSubmit = character => {
         this.setState({characters: [...this.state.characters, character]});
     }
     ```

   * add Form to render with handleSubmit：

     ```js
     return (
         <div className="container">
             <Table
                 characterData={characters}
                 removeCharacter={this.removeCharacter}
             />
             <Form handleSubmit={this.handleSubmit} />
         </div>
     );
     ```

3. add Form operation: 

   * submitForm = () => {} 

     ```js
     submitForm = () => {
         this.props.handleSubmit(this.state);
         this.setState(this.initialState);
     }
     ```

   * add submit button 

     ```js
     <input 
         type="button" 
         value="Submit" 
         onClick={this.submitForm} />
     ```



### Pulling in API Data

利用React lifecycle method ***componentDidMount()*** 获取API DATA

```js
import React, { Component } from 'react';

class App extends Component {
    state = {
        data: []
    };

    // Code is invoked after the component is mounted/inserted into the DOM tree.
    componentDidMount() {
        const url = "https://en.wikipedia.org/w/api.php?action=opensearch&search=Seona+Dancing&format=json&origin=*";

        fetch(url)
            .then(result => result.json())
            .then(result => {
                this.setState({
                    data: result
                })
            });
    }

    render() {
        const { data } = this.state;

        const result = data.map((entry, index) => {
            return <li key={index}>{entry}</li>;
        });

        return <ul>{result}</ul>;
    }
}

export default App;
```

### BUILD AND DEPLOY

1. modify package.json  add homepage

   ```js
   "homepage": "https://taniarascia.github.io/react-tutorial",
   ```




## 需注意的坑

### import {Component}

**Q**

如只import React将会导致 报错

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';

class App extends Component {
    render() {
        return (
            <div className="App">
                <h1>Hello, React!</h1>
            </div>
        );
    }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

```
./src/index.js
  Line 5:  'Component' is not defined  no-undef

Search for the keywords to learn more about each error.
```

**A**

```js
import React, {Component} from 'react';
```



### npx create-react-app错误

**Q**

参照教程 https://www.taniarascia.com/getting-started-with-react/ 使用如下命令：

```bash
npx create-react-app react-tutorial
```

长时间无法安装成功。

**A**

由于国外网站访问问题，建议切换成淘宝NPM镜像，设置方法为：

```BASH
npm config set registry https://registry.npm.taobao.org，
```

设置完成后，再次运行上述命令即可成功。



# [Getting Started with Git by Tania Rascia](https://www.taniarascia.com/getting-started-with-git/)

git基本命令学习， 也可以使用git desktop代替。



