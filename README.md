# REACT NOTES

  
## CONTENTS  
  

- [HOOKS](#HOOKS)
- [SYNTAX](#SYNTAX)  
- [LOCAL STORAGE](#LOCAL-STORAGE)
- [GENERAL NOTES](#GENERAL-NOTES)
<br></br>  
  
![](https://bs-uploads.toptal.io/blackfish-uploads/components/seo/content/og_image_file/og_image/777655/react-context-api-4929b3703a1a7082d99b53eb1bbfc31f.png)

## IMPORTANT 

- Render only returns **one component inside a div**. *If you try to return `<h1>` below the div it will crash*
- custom functions can not add more functions 
- **Never modify a state variable**, first create a copy, modify that and set the state to the copy. 
      
      
Custom props all need to be defined 

```js
<Backdrop />                     // FINE
<Backdrop onClick={myHandler} /> // NOT FINE  (Backdrop does not have onClick prop defined)
```
- `onClick` doesn't exist for this custom func...
- We would need to specify it there as a prop
- `myHandler` is within this class/function

<br></br><br></br><br></br><br></br><br></br>  

## HOOKS

- hooks give you access to lower level features of react outside of the context of a component. 

```js
useSuperPower()
```  

RULE: ONLY CALL AT THE TOP LEVEL OF A FUNCTIONAL COMPONENT   
*(They don't work inside nested functions, loops or anything else)*. 
  
```js
function App(){

useHook();           //CORRECT  

const fun = (){
  useHook();        //INCORRECT
  console.log(fun)
}

}
```  
  
- Exception to the rule is in custom shooks  
  
## HOOKS: UseState  

When `data changes` `re-render the UI`  
We don't assing new  value, we call the second function that assigns new value    

Return value 1: current state i.e. False   
Return value 2: Function to set this value to new one  

```js
const [values, functionToSetValues] = useState([])
```
  
Example:  
  
```js
import { useState } from 'react';

function App(){
  const [count,setCount] = useState(0)

  return(
    <button onClick={()=> setCount(count+1) }>
      {count}
    </button>
  )
}
```
   



## HOOKS: useEffect  

Need to first understand `lifecycle`  
  

- Simplified breakdown  

```js
componentDidMount(){
  // initialized once
}

componentDidUpdate(){
  // State udpated 
}

componentWillUnmount(){
  // Destroyed once
}

```
  
    
Keep this in mind  as we talk about the `useEffect` hook
It allows us to implement logic for all of them within a single function
- It's a function
- That takes a function you define as its first argument
- React will run your function/side effect after it has udpated the dom 
- With the current implementation it will run anytime stateful data changes on component  
- So it runs once on default, then again when state(count) is updated


```js
import { useState } from 'react';

function App(){
  const [count,setCount] = useState(0)

  useEffect(()=>{
    alert('hello side effect!')                 // call anytime there is a change
    return ()=> alert('Goodbye component' )     // call prior to component dismount (destroyed)
  },[] )                                        // specifies the dependencies to call (empty=call once, [count] = when count changes)

  return(
    <button onClick={()=> setCount(count+1) }>
      {count}
    </button>
  )
}
```



- But sometimes we want fine control
- I.e. We want to fetch data once component initialised 
- Then udpate state async once data is fetched
- This would be an infinte loop
  - fetch
  - update state
  - triggers fetch

### Fix infinite loop 

- adding an array of dependencies
- Empty array only runs once 

```js
import { useState } from 'react';

function App(){
  const [count,setCount] = useState(0)

  useEffect(()=>{
    fetch('foo').then(()=> setLoaded(true)  )
  },[count] )                                     // Runs when count changes

  return(
    <button onClick={()=> setCount(count+1) }>
      {count}
    </button>
  )
}
```


### TEAR DOWN
  
- Calls the function returened just before it's destroyed   

```js
  useEffect(()=>{

    alert('hello side effect!')
    return ()=> alert('Goodbye component' )

  } )

```

<br></br><br></br><br></br><br></br><br></br>  

## SYNTAX  

- referring to a function 

```javascript
//incorrect - this executes immediately
onClick={deletehandler()}    
// correct - this executes uponed call
onClick={deletehandler} 
```
  

<br></br><br></br><br></br><br></br><br></br>    

## LOCAL STORAGE 


```js 

const LOCAL_STORAGE_KEY = 'todoApp.todo'

export default function App() {

  // INIT LIST ONCE
  useEffect( ()=>{
    // parse from storage key
    const storedList = JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY))
    if(storedList) setTodos(storedList)
  },[] )

  //  UPDATE ON CHANGE
  useEffect(()=>{
    localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(storedList))
  },[storedList])


```

<br></br><br></br><br></br><br></br><br></br>  


## GENERAL NOTES

- **Components names should start with capital**
- Main page is in the `public/index.html` that gets hijaced and dynamiced by react
- `App.js` needs `export default App;` 
- React is all about small modular components that call others
- JS is imperative `we describe what should happen` 
- React is declarative `we describe the target result` 

Using the main react [Tutorial](https://reactjs.org/tutorial/tutorial.html)

1. React is defined in `index.js` 
2. In `src` folder 

### Example Usage 

- The `shopping list` class can now be called using:
- `<ShoppingList name="Mark" />`
  

```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

```  
  
- Longer way of defining say `<li>` would be to use `createElement()`  method which is bulky and long.  


## STATE 
  
React components can have `state` by setting `this.state` in their constructors. `this.state` should be considered as private to a React component that it’s defined in.   
  
IN THE TUTORIA; Let’s store the current value of the Square in this.state, and change it when the Square is clicked.


## SUPER 

In JavaScript classes, you need to always call `super` when defining the `constructor` of a subclass.  
**All React component classes that have a constructor should start with a super(props) call**
  
EXAMPLE BELOW:  
  
```javascript
// constrcutor in question
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }


  render() {
    return (
      <button className="square" onClick={() => console.log('click')}>
        {this.props.value}
      </button>
    );
  }
}
```


## COMMON MISTAKES 

Notice how with `onClick={() => console.log('click')}`, we’re passing a function as the onClick prop. 
React will only call this function after a click.  
Forgetting `() =>` and writing` onClick={console.log('click')}` is a common mistake, and would fire **every time** the component re-renders.




