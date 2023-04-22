# kothagattu-gunavardhan_FRONTEND
1. Explain what the simple List component does.
List is a React component that is defined in the code. SingleListItem and WrappedListComponent are two further sub-components that make up the component.
A single list item's index, whether or not it is presently chosen, if it has a click handler function, and its text content are all properties that SingleListItem,
a memoized functional component, accepts. Depending on whether the item is chosen or not, the returned list item element displays the text content and changes the
background colour.
Another memoized functional component that accepts an array of objects as parameters is called WrappedListComponent. Additionally, it uses the useState hook to
set the value of the selectedIndex state variable. The selectedIndex state variable is set to null by the useEffect hook whenever the items prop changes. handleClick 
is a function.


2. What problems / warnings are there with code?
(i). useState function returns a list of two items. The first returned value should be vaule and second should be function but here it returns function first and value second.
`const [setSelectedIndex, selectedIndex] = useState();`

(ii). ` <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />`

Here after 'SingleListItem' we need to declare 'key' because to make list unqiue.

(iii). here in `isSelected={selectedIndex}` we need to keep strict equal (===) with index because isSelected is boolean and selectedIndex is a integer i.e useState() return int value.

(iv). `WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};`

here array should be 'arrayof' and shapeof should be 'shape'

(v). '
WrappedListComponent.defaultProps = {
  items: null,
};'
item objects should not be null values should be assigned.


3. fix, optimize, and/or modified code.

```JAVASCRIPT
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';
 // Single List Item
const WrappedSingleListItem = ({index, isSelected, onClickHandler, text}) => {
    return (
      <li
        style={{ backgroundColor: isSelected ? 'green' : 'red'}}
        onClick={()=>onClickHandler(index)}
      >
        {text}
      </li>
    );
  };
  
  WrappedSingleListItem.propTypes = {
    index: PropTypes.number,
    isSelected: PropTypes.bool.isRequired, 
    onClickHandler: PropTypes.func.isRequired,
    text: PropTypes.string.isRequired,
  };
  
  const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({items}) => {

    const [selectedIndex, setSelectedIndex] = useState();

    useEffect(() => {
      setSelectedIndex(null);
    }, [items]);
  
    const handleClick = (index) => {
      setSelectedIndex(index);
    };
  
    return (
      <ul style={{ textAlign: 'left' }}> 
        { items.length ? items.map((item, index) => (
            <SingleListItem
                key = {index}  
                onClickHandler={() => handleClick(index)}
                text={item.text}
                index={index}
                isSelected={index === selectedIndex} 
            />
            )) : []} 
      </ul>
    )
  };
  
  WrappedListComponent.propTypes = {
    items: PropTypes.arrayOf(PropTypes.shape({
      text: PropTypes.string.isRequired,
    })),
  };
  
  WrappedListComponent.defaultProps = {
    items: null,
  };
  
  const List = memo(WrappedListComponent);
  
  export default List;
  ```
