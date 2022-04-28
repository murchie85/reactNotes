# REACT NOTES


[Jump to getting started notes](#Getting-Started-with-Create-React-App)

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



## SYNTAX NOTES  

- referring to a function 

```javascript
//incorrect - this executes immediately
onClick={deletehandler()}    
// correct - this executes uponed call
onClick={deletehandler} 
```
  

## Function notes 

**useState** 
   
We don't assing new  value, we call the second function that assigns new value  

Return value 1: current state i.e. False 
Return value 2: Function to set this value to new one

```js
const [aIsOn, setAIsOFF] = useState(false)
```


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




