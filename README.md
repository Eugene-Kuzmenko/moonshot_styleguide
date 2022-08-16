# Style guide

## Javascript

### Dont write multiple statement into the single line
Reading multple statement written in single line is hard to read and hard to debug

Bad
```javascript
event => {
    setIsOpen(!isOpen); event.preventDefault();
}
```

Good
```javascript
event => {
   setIsOpen(!isOpen);
   event.preventDefault();
}
```

### Dont do comma expressions
Don't separate operator by a comma. There are very few use cases to do it and writing a handle is not it


Bad
```javascript
event => {
    setIsOpen(!isOpen), event.preventDefault();
}
```

Good
```javascript
event => {
   setIsOpen(!isOpen);
   event.preventDefault();
}
```

### Please don't write one letter variable names unless its `i`
It's much easier to read meaningful parameter names, even if you've seen `event` shortened to `e` in some guides

## Typescript

### Avoid optional fields when they're not needed
If you guarantied to have certain value don't make field optional. It would make typisation less useful and force you to do extra null checks

## React

### Dont use useMemo on simple expressions
There is no need to memo logical expression or ternary operator
Memoisation check would be heaview
Just memo stuff that require iteration, like string search or filtering the collection

### Move functions outside the component
If you have a function that doesn't use any of the variables declared within thecomponent without passing them as the parameter, move it outside the component
or maybe into util file
That way it won't be redeclared and you could potentially share it between the component if you encounter a case for it. You could then simply move it into util

### Dont use useCallback, unless you pass the function to a component
useCallback is made to prevent rerenders when you pass callback to component, that using use memo HOC or extends PureComponent. Otherwise it doesn't provide any benefit. So if you need to declare function within the component, then just declare it without useCallback, or, if it doesn't use anything within the component declare it outside


### Avoid nesting
3-4 levels of nesting is fine. Past that code is hard to read and line limit is
hard to mantain

### Split components
If you have have a lot of nesting in your component or it start to excede 200 lines of code, split it into multiple ones. 

Local components are put into ParentComponentPath/components/Component
If you have styles that only concern that component move them into its own module

### Avoid magic numbers
Numbers and colors are to be moved into the constants

### Constants
Constants are written in SHOUT_CASE

### Use string interpolation
Use string interpolation syntax instead of concatenation

Good:
```javascript
`${HEIGHT} px`
```

Bad:
```javascript
HEIGHT + ' px'
```

### Dont use array index as key
React uses key to decide which element corresponds to which between old version of the dom and the new one
If you will use index and the array you're mapping will change, it will get it wrong and all components whose values
were shifted would end up rerendering

## SCSS

### Don't nest classess
When using modules don't nest classess unless you specifically need extra
specificity to override third party class or to reuse class in
certain context within the component

It becomes hard to read, to refactor and to override if you do this
problem is increased by 4 spaces indentation set for editor project

### Location of the component is not part of the component
Avoid putting margins or position  within the components outermost tag. Instead pass className with required margin style. That way it's more easy to reuse component 
