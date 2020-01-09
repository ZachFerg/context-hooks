Following along with The Net Ninjas [React Context and Hooks Tutorial](https://www.youtube.com/playlist?list=PL4cUxeGkcC9hNokByJilPg5g9m2APUePI).

## Lesson 1: Introduction to course.

* Setting up project to showcase the context API in React
* Installed **'Simple React Snippets'** for VS Code

> **imr**	    Import React
> **imrc**	    Import React / Component
> **impt**	    Import PropTypes
> **cc**	    Class Component
> **ccc**	    Class Component With Constructor

## Lesson 2: What is the Context API

* #### Context API:
> Shares state within a component tree (Can be done with props, but gets messy).
> Can be used alongside hooks and makes sharing data between components much easier.

#### [React Context Documentation](https://reactjs.org/docs/context.html)

> Context is designed to share data that can be considered “global” for a tree of React components, such as the current authenticated user, theme, or preferred language.

If you ever are unsure wether you should use context, ask yourself: **Is the data that I want to share global, and is it gonna be used in several of the child components?**

> Context is not a catch-all for every situation, just for global state thats shared between many different components in a tree.
> This is also a good time to download the **React Developer Tools** Extension for Chrome

## Lesson 3: Adding a Context & Provider.

> Creating a new context (ThemeContext.js) and providing it to our components
> When you create a context, you also need to create a provider (a tag that surrounds whichever components we want to use that context)

*ThemeContext.js*
```javascript
<ThemeContext.Provider value={{...this.state}}>
    {this.props.children}   // wraps the two components with this Provider
</ThemeContext.Provider>
```

*App.js*
```javascript
<div className="App">
    <ThemeContextProvider>
        <Navbar />
        <BookList />
    </ThemeContextProvider>
</div>
```
When we surround the data using a context component, Navbar and Booklist are attached to the props of this component.

## Lesson 4: Accessing Context (part 1)
> Accessing our context data using the static property ```contextType```

```javascript
const { isLightTheme, light, dark } = this.context;
const theme = isLightTheme ? light : dark ;
```
Everytime the state changes, anything consuming the context ```ThemeContext``` and takes in that value, that will update with a new value. Because that value is being updated in both BookList and Navbar, when ```isLightTheme``` is updated from true to false, the [ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) will be returning something different, Light when true or Dark when False.

## Lesson 5: Accessing Context (part 2)
> Another way of accessing our context data using a context consumer. This way can be used in both functional and class components.

When creating a context, not only are you given a ```Provider```, you're also given a ```Consumer```

```javascript
return (
    <ThemeContext.Consumer>{(context) => {
        const { isLightTheme, light, dark } = context;
        const theme = isLightTheme ? light : dark ;
        return(
            <nav style={{ background: theme.ui, color: theme.syntax}}>
                <h1>Context App</h1>
                <ul>
                    <li>Home</li>
                    <li>About</li>
                    <li>Contact</li>
                </ul>
            </nav>
        )
    }} </ThemeContext.Consumer>
);
```

All we're doing is using the ```Consumer``` of the ```ThemeContext``` we created. Inside that we're using a function which takes in that context object as a paraameter. Inside this function we have access to this context object, destructuring some information or constants from that context object and than we're setting the current theme.

**After seeing two ways of using it, which one to use?**

* **context type approach:** simpler way when using a class component
* **context consumer approach:** Can be used in functional or class components. It can also consume multiple context in one component.

## Lesson 6: Updating Context Data
```javascript
import React, { Component } from 'react';
import { ThemeContext } from '../contexts/ThemeContext';

class ThemeToggle extends Component {
    static contextType = ThemeContext;
    render() {
        const { toggleTheme } = this.context; 
        return ( 
            <button onClick={toggleTheme}> Toggle Theme.</button>
         );
    }
}
export default ThemeToggle;
```
Creating a button that will allow us to update the context data (light or dark theme) from our components