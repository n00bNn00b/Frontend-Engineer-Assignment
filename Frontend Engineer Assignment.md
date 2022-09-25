# Frontend Engineer Assignment

In our daily React coding life we face many problems, create bugs unintentionally. We solve them step by step by watching the errors and warnings and fix the bugs.
Here is an example React code which has some bugs and need to fix them.

# Problem

```javascript
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [setSelectedIndex, selectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.array(
    PropTypes.shapeOf({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: null,
};

const List = memo(WrappedListComponent);

export default List;
```

## Overview of the List Component

On the above Code We have some bugs and need to fix them.
At first glance let us discuss about the `List` component.

1. Explain what the simple `List` component does.

The List component receives a prop from the main component as a variable named items which is in an array of objects. Inside the component it has a set state function along with an initial state of selectedIndex. Inside of the useEffect hook its state is set as null and hook's dependency is the items prop which is received from the Main or App.js. There is an event handler function which has a parameter of index and inside of this function setState function's parameter is index which will be used in another component.

After that it has an unordered list and inside it items are mapped and sent its objects inside of the SingleListItem component. It is also checked if the items prop's property type is an array of object or not and the object of text is checked that it is required.

It has also default property as null.

In shortly List component receives items as a prop which is an array of objects and sends the objects and the click handler functions as props to SingleLisItem component.

## Problems / Warnings of the above Code

2.  What problems / warnings are there with code?

###

1. `shapeOf()`- There is no `shapeOf` function in `"prop-types"`. Instead it will be `shape()`
2. `array()`- As we will be passing array of objects, with `array()` we can not check the type. When we are going to check its property type, it will be `arrayOf()`
3. The State declaration:

```javascript
const [setSelectedIndex, selectedIndex] = useState();
```

In React `setState` is a function by which we use to set an initial or new state for the desired state variable. Instead it will be:

```javascript
const [selectedIndex, setSelectedIndex] = useState();
```

4. There is no unique key for the SingleListItem component.
5. ` Warning: Cannot update a component ( ``WrappedListComponent`` ) while rendering a different component (``WrappedSingleListItem``). To locate the bad setState() call inside ``WrappedSingleListItem ` - This error occured in `WrappedListComponent ` because of the event handler. When the event handler function was called there was no arrow function. To avoid this warning using arrow function will fix the issue.
   Solution will be:

```javascript
<li
  key={index}
  style={{ backgroundColor: isSelected ? "green" : "red" }}
  onClick={() => onClickHandler(index)}
>
  {text}
</li>
```

6. In List component we see `isSelected={selectedIndex}`- if we send isSelected prop as this, it will send a number because selectedIndex is a number and the application will throw an error because it requires boolean value not the numeric value. Instead I have come through a solution `if the selectedIndex is even number, it will be true and if the selectedIndex is odd number it will be false`.
   The solution is like:

```javascript
<SingleListItem
  key={index}
  onClickHandler={() => handleClick(item.index)}
  text={item.text}
  index={item.index}
  isSelected={selectedIndex % 2 === 0 ? true : false}
/>
```

## The Whole Solution and modification with styles

3. Please fix, optimize, and/or modify the component as much as you think is necessary.

### Solution

```javascript
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{
        backgroundColor: isSelected ? "green" : "red",
        margin: "auto",
        marginTop: "5px",
        padding: "10px",
        width: "100px",
        borderRadius: "10px",
        cursor: "pointer",
        color: "white",
        textAlign: "center",
      }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };
  // console.log(selectedIndex);

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(item.index)}
          text={item.text}
          index={item.index}
          isSelected={selectedIndex % 2 === 0 ? true : false}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: null,
};

const List = memo(WrappedListComponent);

export default List;
```

And I used `items` as an array of objects in `App.js`. Then I have passed the array as props to the `List` component.

```javascript
import List from "./List";

export default function App() {
  const items = [
    {
      index: 1,
      text: "ON",
    },
    {
      index: 2,
      text: "OFF",
    },
  ];
  return (
    <div>
      <h1>Prop Types Bug Fixing</h1>
      {/* <List items={items} /> */}
      <List items={items} />
    </div>
  );
}
```
