# Frontend Engineer Assignment

In our daily React coding life we face many problems, create bugs unintentionally. We solve them step by step by watching the errors and warnings and fix the bugs.
Here is an example React code which has some bugs and need to fix them.

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

## Overview

On the above Code We have some bugs and need to fix them.
At first glance let us discuss about the `List` component.

1. Explain what the simple `List` component does.

The List component receives a prop from the main component as a variable named items which is in an array of objects. Inside the component it has a set state function along with an initial state of selectedIndex. Inside of the useEffect hook its state is set as null and hook's dependency is the items prop which is received from the Main or App.js. There is an event handler function which has a parameter of index and inside of this function setState function's parameter is index which will be used in another component.

After that it has an unordered list and inside it items are mapped and sent its objects inside of the SingleListItem component. It is also checked if the items prop's property type is an array of object or not and the object of text is checked that it is required.

It has also default property as null.

In shortly List component receives items as a prop which is an array of objects and sends the objects and the click handler functions as props to SingleLisItem component.
