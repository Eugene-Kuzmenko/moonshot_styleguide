# Style guide

## React

### Dont useMemo on simple expressions
There is no need to memo logical expression or ternary operator
Memoisation check would be heaview
Just memo stuff that require iteration, like string search or filtering the collection

### Move functions outside the component
If you have a function that doesn't use any of the variables declared within thecomponent without passing them as the parameter, move it outside the component
or maybe into util file
That way it won't be redeclared and you could potentially share it between the component if you encounter a case for it. You could then simply move it into util

### Dont useCallback, unless you pass the function to a component
useCallback is made to prevent rerenders when you pass callback to component, that using use memo HOC or extends PureComponent. Otherwise it doesn't provide any benefit. So if you need to declare function within the component, then just declare it without useCallback, or, if it doesn't use anything within the component declare it outside


### Avoid nesting
3-4 levels of nesting is fine. Past that code is hard to read and line limit is
hard to mantain

### Split components
If you have have a lot of nesting in your component or it start to excede 200 lines of code, split it into multiple ones. 

Local components are put into ParentComponentPath/components/Component
If you have styles that only concern that component move them into its own module

## SCSS

### Don't nest classess
When using modules don't nest classess unless you specifically need extra
specificity to override third party class or to reuse class in
certain context within the component

It becomes hard to read, to refactor and to override if you do this
problem is increased by 4 spaces indentation set for editor project

### Location of the component is not part of the component
Avoid putting margins or position  within the components outermost tag. Instead pass className with required margin style. That way it's more easy to reuse component 
