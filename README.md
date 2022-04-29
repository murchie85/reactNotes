# REACT NOTES

  
## CONTENTS  
  

- [HOOKS](#HOOKS)
- [SYNTAX](#SYNTAX)  
- [LOCAL STORAGE](#LOCAL-STORAGE)
- [MORE NOTES](#GENERAL-NOTES)
<br></br>  
  
![](https://bs-uploads.toptal.io/blackfish-uploads/components/seo/content/og_image_file/og_image/777655/react-context-api-4929b3703a1a7082d99b53eb1bbfc31f.png)

## GENERAL 

- **RENDER ONLY RETURNS ONE COMPONENT**. *If you try to return `<h1>` below the div it will crash* So use `<>` instead. 
- custom functions can not add more functions 
- **Never modify a state variable**, first create a copy, modify that and set the state to the copy. 
- **Components names should start with capital**
- Main page is in the `public/index.html` that gets hijaced and dynamiced by react
- `App.js` needs `export default App;` 
- React is all about small modular components that call others
- JS is imperative `we describe what should happen` 
- React is declarative `we describe the target result` 
      
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


## HOOKS: useContext
  
( `createContext first`, then useContext deep down without needing to pass kid all the way there with `useContext`)

  
- Allows you to work with reacts `context api` 
- A mechanism which allows you to share or scope values thoughout the entire component tree.
- to share mood accross multiple disconnected components we crate context 
- one comp might be happy so we use a `context provider`  to scope the `happy mood` there 


```js
const moods = {
  happy: '😀'
  sad:   '😢'
}

const MoodContext = createContext(moods);

function App(props){
  return(

    // inherits the value and doesn't need to pass props down to chilren
    <MoodContext.Provider value={moods.happy}>
      <MoodEmoji />
    </MoodContext.Provider>
    
  );
}

// this updates anytime it updates further up
function MoodEmoji(){
  const mood = useContext(MoodContext);    // Consume value from nearest parent provider 

  return <p>{ mood } </p>

}


```

It used to be longer using `context.consumer` i.e. 

```js


function MoodEmoji(){
  return <MoodContext.Consumer>
    { ({mood}) => <p>{mood}</p>  }
    </MoodContext.Consumer>
}
```


<br></br><br></br><br></br><br></br><br></br>  


## HOOKS: useRef  
 
Allows you to create a mutalbe object 
That keeps the same reference between renders
Like setState, but it `DOESN'T TRIGGER A RERENDER WHEN VALUE CHANGES`
So clicking button below wont increment because it wont trigger a rerender


```js

function App(){
  const count = useRef(0);

  return(
    <button onclick={ () => count.curent++ }>
      {count.current}
    </button>

  )
}


```


  
- A more common use case is to `grab html elements from the DOM`  

```js

function App(){

  const myBtn = useRef(null);
  
  // programatically clicks the button
  const clickIt = ()=> myBtn.current.click()

  return(
    <button ref={myBtn}> <button>
  )
}


}



```

<br></br><br></br><br></br><br></br><br></br>  


## HOOKS: useReducer()

- similar to `setState()` 
- But different way to manage state  
- Uses `redux pattern`  
Instead of updating state directly:  
  
1. set `useReducer`
2. give it a function i.e. reducer
3. set it to the value and the hook ref name
4. hookref name can be  called which has a type and optional payload
5. The `useReducer` function you gave it passes state and action

The dispatcher is an action that has a `type` and an optional data payload


```js

function App(){
  
  // first is the value, second is function that dispatches an action
  const [state, dispatch] =  useReducer(reducer,0);  // reducer is the function we define, 0 is the initial state
  
  return(
    <>
     Count: {state}
     <button onClick={ ()=> dispatch({type:'decriment'}) }> <button>
    </>
  )
  }

```

```js

function reducer(state,action){
  switch(action.type){
    case 'increment':
      return state + 1;
    case 'decriment':
      return state - 1;
    default:
      throw new Error();
  }
}

```  
  
This helps if program gets too big (an alternate for setState)

<br></br><br></br><br></br><br></br><br></br>  





## HOOKS: useMemo()

- Reduces required compute
- `caution` use only for expensive calculations 


imagine we have a counter 
but we compute another one `expensiveCount`


```js
// only when count chanegs 
const expensiveCount = useMemo( ()=>{
  return count**2;
}, [count])

```


<br></br><br></br><br></br><br></br><br></br>  


## HOOKS: useCallback()

- like `useMemo()` but when you want to memoize an entire function
  
when you define a function in a component 
a new function object is created each time a component is re-rendered
sometimes this is performance intensive  
  
- common use case is when you pass function to multiple children components 
- **PREVENTS RERENDER FOR EVERY CHILD**   cus they will use same object.  
  



```js
function App(){
  const [count,setCount] = useState(60);
  const showCount = useCallback( ()=> {
    alert(`Count ${count}`)
  }, [count])

  return <> <someChild handler={showCount}>
}

```
  

<br></br><br></br><br></br><br></br><br></br>  


## HOOKS: useImperativeHandle()  
  
- confusing  
- but allows you to modify a lib that you share with others 
- rare use 

if you build reusable component
you may need access to underlying DOM 
then forward so consumers of lib can use 

you could grab it using `useRef` from earlier
then wrap it using `forwardRef` to make it available when someone uses the component.  
  
`useImperativeHandle()` comes in if you want to `change the behaviour` of the exposed ref 
  

<br></br><br></br><br></br><br></br><br></br>  


## HOOKS: useLayoutEffect()    
  
- like useEffect
- will run after rendering comp
- but before its painted to screen

**blocks visual updates until your callback is finished**  
 

<br></br><br></br><br></br><br></br><br></br>  


## HOOKS: useDebugValue()

- only good for custom hooks  
- Makes it possible to define custom debug values
- that get shown in react debug in dev console  
  
```js
useDebugValue(displayName ?? 'loading....')
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


## MORE NOTES



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




