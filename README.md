# Redux Reducers

## Introduction

A reducer specifies how the state of an application should be updated in response to a dispatched action. A reducer is a pure function that takes the previous state and an action and returns the new state.

Inside a reducer you should never:

1. Mutate its arguments.
2. Make API calls or routing transitions.
3. Call non-pure functions, such as Math.random().

## Objects

To correctly update an object every level of nesting needs to be copied and updated. This must be done to avoid accidental mutation of the state.

Updating `state.first[someId].second`:

```Javascript
function updateField(state, action) {
  return {
    ...state,
    first : {
      ...state.first,
      [action.someId] : {
        ...state.first[action.someId],
          second : action.someValue
      }
    }
  }
}
```

## Arrays

An array is normally updated using `.push()`, `.splice()` etc. In a reducer we do not want to modify the state directly, so we must first make a copy of the array. We can not use `.push()` as this returns a length.

Values can then be inserted/removed as follows:

### Add/Remove Item

```javascript
function insertItem(array, action) {
  let newArray = array.slice();

  return newArray.splice(action.index, 0, action.item);
}

function removeItem(array, action) {
  let newArray = array.slice();

  return newArray.splice(action.index, 1);;
}
```

remove could also be written as:

```javascript
function removeItem(array, action) {
  return array.filter( (item, index) => index !== action.index);
}
```

### Update Item

Updating can be achieved using `.map()`:

```javascript
function updateObjectInArray(array, action) {
  return array.map( (item, index) => {
    if(index !== action.index) {
      // This isn't the item we care about - keep it as-is
      return item;
    }

    // Otherwise, this is the one we want - return an updated value
    return {
      ...item,
      ...action.item
    };    
  });
}
```
