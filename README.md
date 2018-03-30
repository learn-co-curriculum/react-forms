# React Controlled Components

## Overview 

In this lesson, we'll discuss components and how to use components to set and get values in form elements. 

## Objectives
1. Explain how React uses `value` on, e.g., `<input>`
2. Check whether a component is controlled or uncontrolled
3. Describe strategies for using controlled components
4. Use controlled inputs to validate values
5. Distinguish between `value` and `defaultValue` in a React controlled component
   
## Code Along 

If you want to code a long there is starter code in the `src` folder. Make sure to run `npm install && npm start` to see the code in the browser.

## Form basics
![You'll be writing a lot of forms in React.](http://s2.quickmeme.com/img/95/95a52393032e643e9817eda6d7485cc770865ea6929278386c8e723a6ca42adc.jpg)

Forms in React are fairly straight-forward and similar to regular HTML elements, albeit with a few changes that we need to be aware of.

Form elements include `<input>`, `<textarea>`, `<select>`, and `<form>` itself. When we talk about inputs in this lesson, we broadly mean the form elements (`<input>`, `<textarea>`, `<select>`) and not always specifically just `<input>`.

To control the value of these inputs, we use a prop specific to that type of input:

- For `<input>` and `<textarea>`, we use `value`
- For `<input type="checkbox">` and `<input type="radio">`, we use `checked`
- For `<option>`, we use `selected`

To listen for changes to the value of an input, we simply pass in the `onChange` prop with a callback function. Default values are set using either the regular value prop (`value`, `checked`, ...) _or_ the `defaultValue`/`defaultChecked` prop. This depends on if we're using an uncontrolled or controlled component. Read on to find out the difference!

## Uncontrolled vs controlled components
![Kpop](https://media.giphy.com/media/QcnfLD17Ebt28/giphy.gif)

React provides us with two ways of setting and getting values in form elements. These two methods are called _uncontrolled_ and _controlled_ components. The differences are subtle, but it's important to recognize them — and use them accordingly (spoiler: most of the time, we'll use _controlled_ components).

The quickest way to check if a component is controlled or uncontrolled is to check for `value` or `defaultValue`. If the component has a `value` prop, it is controlled (the state of the component is being controlled by React). If it doesn't have a `value` prop, it's an uncontrolled component. Uncontrolled components can optionally have a `defaultValue` prop to set its initial value. These two props (`value` and `defaultValue`) are _mutually exclusive_: a component is either controlled or uncontrolled, but it cannot be both.

### Uncontrolled component
In uncontrolled components, the state of the component's value is kept in the DOM itself — in other words, the form element in question (e.g. an `<input>`) has its _own internal state_. To retrieve that value, we would need direct access to the DOM component that holds the value, _or_ we'd have to add an `onChange` handler.

To set an initial value for the element, we'd use the `defaultValue` prop. We can't use the `value` prop for this: we're not using state to explicitly store its value, so the component would never update its value anymore (since we're rendering the same thing).

### Controlled component
In controlled components, we explicitly set the value of a component, and update that value in response to any changes the user makes to the value of that component. That might sound a little wonky, but when you see the code, it'll become much clearer:

```js
// src/components/ControlledInput.js
import React from 'react';

class ControlledInput extends React.Component {
  constructor() {
    super();
    
    this.state = {
      value: '',
    };
  }
  
  handleChange = event => {
    this.setState({
      value: event.target.value,
    });
  }

  render() {
    return (
      <input 
        type="text" 
        value={this.state.value} 
        onChange={this.handleChange} 
      />
    );
  }
}

export default ControlledInput;


// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';

import ControlledInput from './components/ControlledInput';

ReactDOM.render(
  <ControlledInput />,
  document.getElementById('root')
);
```

As you can see, we can easily define the initial value by setting the initial `value` property on the state to whatever we want. 

Using a controlled component is the preferred way to do things in React — it allows us to keep _all_ component state in the React state, instead of relying on the DOM to retrieve the element's value through its internal state. Whenever our state changes, the component re-renders, rendering the input with the new updated value. If we don't update the state, our input wouldn't update when the user would type. In other words, we need to update our input's state _programmatically_.

It might seem a little counterintuitive that we need to be so verbose, but this actually opens the door to additional functionality. For example, let's say we want to write an input that only takes in a number (let's pretend there is no `<input type="number">`). We can now validate the data the user enters _before_ we set it on the state, allowing us to block any invalid values. If the input is invalid, we simply avoid updating the state, preventing the input from updating. We could optionally set another state property (for example, `isInvalidNumber`). Using that state property, we can show an error in our component to indicate that the user tried to enter an invalid value.

If we tried to do this using an uncontrolled component, the input would be entered regardless, since we don't have control over the internal state of the input. In our `onChange` handler, we'd have to roll the input back to its previous value, which is pretty tedious!

## Resources
- [React Forms](https://facebook.github.io/react/docs/forms.html)
- [Controlled vs Uncontrolled](https://www.sitepoint.com/video-controlled-vs-uncontrolled-components-in-react/) - Video

<p class='util--hide'>View <a href='https://learn.co/lessons/react-forms'>Forms</a> on Learn.co and start learning to code for free.</p>
