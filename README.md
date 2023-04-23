
# Frontend Engineer Assignment
<a href="https://v209fs.csb.app/ ">Demo(Sandbox)- https://v209fs.csb.app/</a>

```
SteelEye

Deployed Link - https://v209fs.csb.app/
Sandbox Link  - https://codesandbox.io/s/boring-bassi-v209fs?file=/src/App.js
Github Link      - https://github.com/Vivek1898/Vivek_Singh_Frontend


Submitted by 
viveksingh27795@gmail.com

```

Q1- Explain what the simple List component does.
```
The simple List component is a React component that takes an array of items as a prop, and renders 
them as a list of clickable items. Each item is represented by a SingleListItem component, 
which is rendered as an <li> element, and has a background color that changes depending on whether 
it is selected or not. The List component maintains the state of the selected item using the 
useState hook, and updates it when an item is clicked.
```

Q2 . What problems / warnings are there with code?
```
One problem with the code is that the isSelected prop in the SingleListItem component is not being 
properly passed as a boolean value. Instead, it is being passed the selectedIndex state, 
which is an integer. This will cause the background color of all items to be green, 
even when they are not selected. To fix this, the isSelected prop should be passed
as a boolean value, like this:
```
```
isSelected={selectedIndex === index}
```

```
Another issue is that the defaultProps for the WrappedListComponent
is set to null, which does not make sense as it should be an empty array.

Furthermore, the handleClick function is being called immediately,
when the component is rendered, rather than when an item is clicked.
To fix this, it should be wrapped in an anonymous function, like this:



onClickHandler={() => handleClick(index)}




Lastly, since the SingleListItem component is wrapped in the memo HOC,
the index prop should not be included in the dependency array of the
handleClick function, as it will cause unnecessary re-renders. Instead,
the handleClick function should simply take the index as a parameter, like this:


const handleClick = (index) => {
  setSelectedIndex(index);
};
```

Q3. Please fix, optimize, and/or modify the component as much as you think is necessary.

```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item used
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
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
  text: PropTypes.string.isRequired
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

  return (
    <ul style={{ textAlign: "left" }}>
      {items ? (
        items.map((item, index) => (
          <SingleListItem
            key={index}
            onClickHandler={() => handleClick(index)}
            text={item.text}
            index={index}
            isSelected={selectedIndex === index}
          />
        ))
      ) : (
        <div>Loading...</div>
      )}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired
    })
  )
};

WrappedListComponent.defaultProps = {
  items: [
    { text: "first item" },
    { text: "second item" },
    { text: "third item" },
    { text: "fourth item" }
  ]
};

const List = memo(WrappedListComponent);

export default List;
```


