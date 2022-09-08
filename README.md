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

### Avoid getters for expensive calculations
It's preferrable that you write a method for expensive calculation, as it would indicate that we're starting a something long instead of just getting value of the field

Bad
```javascript
    get prime {
        return calculatePrime();
    }
```


Good
```javascript
    getPrime() {
        return calculatePrime();
    }
```

### Don't copy results of the array `filter`, `map` and simillar methods
Those methods already return copies, so there is no need to copy them

Bad
```javascript
const filteredTabs = [...tabs.filter((tab) => tab.tabKey !== targetKey)];
```

Good
```javascript
const filteredTabs = tabs.filter((tab) => tab.tabKey !== targetKey);
```

### Prefer `.forEach` to `.reduce` if you need to push into array
It's easier to read if initial array initialization is at the start and doesn't cause us to break the rule about mutating parameters

Bad
```javascript
const componentConfigs = components.reduce((acc, component, order) => {
  if (!component.isActive) return;
  acc.push({ ...component, order });
}, []);
```

Good
```javascript
const componentConfigs = [];
components.forEach((component, order) => {
  if (!component.isActive) return;
  componentConfigs.push({ ...component, order });
});
```

## With multiline expession start new line with an operator
It makes it easier to find operator and looks cleaner

Bad
```
const popupBottomPosition = (
    searchInputBounds.y +
    searchInputLength +
    index * itemLength + 
    Math.min(groupedOptions[category].length, 440) *
    itemLength
)
```

Good
```
const popupBottomPosition = (
    searchInputBounds.y
    + searchInputLength
    + index * itemLength 
    + Math.min(groupedOptions[category].length, 440)
    * itemLength
)
```

Bad
```
const isPointWithin = (
    point.x < topRight.x &&
    point.y < topRight.y &&
    point.x > bottomLeft.x &&
    point.y > bottomLeft.y
)
```

Good
```
const isPointWithin = (
    point.x < topRight.x
    && point.y < topRight.y
    && point.x > bottomLeft.x
    && point.y > bottomLeft.y
)
```

## Typescript

### Avoid optional fields when they're not needed
If you guarantied to have certain value don't make field optional. It would make typisation less useful and force you to do extra null checks

### Prefer named interfaces, especially if the type is repeated
It isn't really readable when you define inline types. Inline object types are looking very simillar to object definition. If used in array, inline object types make array sign easily missed. If you define interace, it would also help you avoid repeating types and add purpose to the type

Bad
```typescript
const all = {
  ...reduceAssets(
    components as {
      assets: { id: number; uuid: string; name: string }[];
    }[]
  ),
}
```

Good
```typescript
interface Asset {
  id: number; 
  uuid: string;
  name: string
}

const all = {
  ...reduceAssets(components as Asset[]),
}
```

### Use shorthand syntax if variable name matching the field name
In most cases object shorthand is more readable, since there are less noise. Also it allows shorter object definition

Bad
```javascript
const object = { field: field };
```

Good
```javascript
const object = { field };
```

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

Bad:
```javascript
HEIGHT + ' px'
```

Good:
```javascript
`${HEIGHT} px`
```

### Dont use array index as key
React uses key to decide which element corresponds to which between old version of the dom and the new one
If you will use index and the array you're mapping will change, it will get it wrong and all components whose values
were shifted would end up rerendering

### Multiple classnames
For multiple classnames use classnames library. 
Since we've started importing it as `cx`, import it as such
Although you can, there is no need to pass them as an array. It unnecessary increases clutter

Bad
```javascript
<div className={`some-global ${styles.button} {styles.primary}`} />
```

Bad
```javascript
<div className={cx(['some-global', styles.button, styles.primary])} />
```

Good
```javascript
<div className={cx('some-global', styles.button, styles.primary)} />
```

### Conditional classnames
When using classnames function, pass object to determine which styles apply conditionally
Field is a class name, value is a condition. Dont conditionally dont use "and" operator to pass field name to `styles` object conditionally or to pass class name to `classname` function conditionally
Don't pass multiple objects to create multiple condition

Bad
```jsx
<div 
  className={cx(
    styles.button,
    isPrimary && styles.primary,
  )}
/>
```

Bad
```jsx
<div
  className={cx(
    styles.button,
    { [styles.primary]: isPrimary },
    { [styles.big]: isBig },
  )}
/>
```

Very Bad
```jsx
<div 
  className={cx(
    styles.name,
    styles[`${isPrimary && 'primary'}`],
  )}
/>
```

Good
```jsx
<div
  className={cx(
    styles.button,
    {
      [styles.primary]: isPrimary,
      [styles.big]: isBig,
    }
  )}
/>
```

Good
```jsx
<div
  className={cx(styles.button, {
    [styles.primary]: isPrimary,
    [styles.big]: isBig,
  })}
/>
```

### Don't pass hooks into props
It's becomes hard to modify, read and debug, if hook is passed into props, instead of being on the 
top level
For example if you need to render the block conditionally in the future, you would need to move the hook out first

Bad
```jsx
return (
  <button 
    onClick={useCallback(() => {
      onClick();
    }, [onClick])}
  />
)
```

Good
```jsx
const handleClick = useCallback(() => {
  onClick();
}, [onClick]);

return (
  <button onClick={handleClick} />
)
```

### Dont start functions with the `use` unless they are hooks
Word use is reserved for hooks. That way eslint validates that hooks are used properly and it just gets confusing if you start non hooks with use. It loses meaning

Bad
```js
const result = await useUpdateBulkEditorAssets(
  this.client,
  data
);
```

Good
```js
const result = await updateBulkEditorAssets(
  this.client,
  data
);
```

### *Do not* mutate props
It will cause issues that are hard to debug. It goes against how React is expected to work
If you need to mutate a field during render for some reason, then the thing you're assigning probably should be declared in the component above

Bad
```javascript
const { editorManager } = props;

editorManager.cms = cms;
```

### There should be only one file per component
It gets very confusing and hard to navigate if there are multiple components in a single file

### Don't pass props with wrapper implicitly using `React.cloneElement`
It would make code harder to understand and debug, when by some magic wrapper affects child elements in unexpected manner
If you need to access something from wrapper in the child, better to wrap child into render function

Bad
```jsx
// MagicWrapper.jsx
const MagicWrapper = ({ children })) => (
  React.cloneElement(children, { onClick: () => {} })
);

// Component.jsx
const Component => (
  <MagicWrapper>
    <button>Ok</button>
  </MagicWrapper>
);
```

Good
```jsx
// MagicWrapper.jsx
const MagicWrapper = ({ children })) => (
  children({ onClick: () => {} }))
);

// Component.jsx
const Component => (
  <MagicWrapper>
    {({ onClick }) => (
      <button onClick={onClick}>Ok</button>
    )}
  </MagicWrapper>
);
```

Good
```jsx
// MagicWrapper.jsx
const MagicWrapper = ({ render })) => (
  render({ onClick: () => {} }))
);

// Component.jsx
const Component => (
  <MagicWrapper
    render={({ onClick }) => (
      <button onClick={onClick}>Ok</button>
    )}
  />
);
```

### Pass string constants without curly braces
There is no need to wrap strings in curly braces if you pass string dirrectly

Bad
```jsx
<Button variant={"primary"} />
```

Good
```jsx
<Button variant="primary" />
```

### Avoid passing objects
If you need to pass only a few props and especially if you create object anyway, it's better to pass fields as individual props instead of object. Also it would make component more flexible

Bad
```jsx
<ConditionalLink
    providerVisitLink={{
        text: providerVisitLink.text,
        link,
    }}
/>
```

Good
```jsx
<ConditionalLink
    text={providerVisitLink.text}
    link={link}
/>
```

## Styles

### Don't nest classess needlessly
When using modules don't nest classess unless you specifically need extra
specificity to override third party class or to reuse class in
certain context within the component

It becomes hard to read, to refactor and to override if you do this
problem is increased by 4 spaces indentation set for editor project

### Location of the component is not part of the component
Avoid putting margins or position  within the components outermost tag. Instead pass className with required margin style. That way it's more easy to reuse component 

### Avoid using "-" in class names in scss modules
You would need to use styles object to get classname from scss module. It would be cleaner if you wouldn't need to access it with \[\]. Apply js naming conventions since you would use it in js

Bad
```scss
.no-text {
  //...
}
```

Good
```scss
.noText {
  //...
}
```

### For fragments in styled components use `css` wrapper
It would signal IDE that contents of the string 

Bad
```javascript
const Component = styled.div`
  ${props => `
    color: ${props.colors.red},
  `}
`
```

Good
```javascript
const Component = styled.div`
  ${props => css`
    color: ${props.colors.red},
  `}
`
```

