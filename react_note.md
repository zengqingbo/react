# [Getting Started by Tania Rascia](https://www.taniarascia.com/getting-started-with-react/)

## HINT

### JSX

JSX is actually closer to JavaScript, not HTML, so there are a few key differences to note when writing it.

- `className` is used instead of `class` for adding CSS classes, as `class` is a reserved keyword in JavaScript.
- Properties and methods in JSX are camelCase – `onclick` will become `onClick`.
- Self-closing tags *must* end in a slash – e.g. `<img />`



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



