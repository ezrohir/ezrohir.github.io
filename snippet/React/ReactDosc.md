### JSX in Depth
#### HTML Tags vs. React Components
React can either render HTML tags (strings) or React components (classes).

To render an HTML tag, just use lower-case tag names in JSX:

```
var myDivElement = <div className="foo" />;
ReactDOM.render(myDivElement, document.getElementById('example'));
```

To render a React Component, just create a local variable that starts with an upper-case letter:
```
var MyComponent = React.createClass({/*...*/});
var myElement = <MyComponent someProperty={true} />;
ReactDOM.render(myElement, document.getElementById('example'));
```

#### The Transform
React JSX transforms from an XML-like syntax into native JavaScript.
XML elements, attributes and children are transformed into arguments that are passed to `React.createElement`.

```
var Nav;
// Input (JSX):
var app = <Nav color="blue" />;
// Output (JS):
var app = React.createElement(Nav, {color:"blue"});
```

JSX also allows specifying children using XML syntax:
```
var Nav, Profile;
// Input (JSX):
var app = <Nav color="blue"><Profile>click</Profile></Nav>;
// Output (JS):
var app = React.createElement(
  Nav,
  {color:"blue"},
  React.createElement(Profile, null, "click")
);
```

#### JavaScript Expressions
```
// Input (JSX):
var person = <Person name={window.isLoggedIn ? window.name : ''} />;
// Output (JS):
var person = React.createElement(
  Person,
  {name: window.isLoggedIn ? window.name : ''}
);
```

#### Boolean Attributes
Omitting the value of an attribute causes JSX to treat it as `true`. To pass `false` an attribute expression must be used. This often comes up when using HTML form elements, with attributes like `disabled`, `required`, `checked` and `readOnly`.
```
// These two are equivalent in JSX for disabling a button
<input type="button" disabled />;
<input type="button" disabled={true} />;

// And these two are equivalent in JSX for not disabling a button
<input type="button" />;
<input type="button" disabled={false} />;
```

#### Child Expressions
```
// Input (JSX):
var content = <Container>{window.isLoggedIn ? <Nav /> : <Login />}</Container>;
// Output (JS):
var content = React.createElement(
  Container,
  null,
  window.isLoggedIn ? React.createElement(Nav) : React.createElement(Login)
);
```

### 浏览器中的工作原理
#### Refs和getDomNode()
为了和浏览器交互，你将需要对DOM节点的引用。每一个挂载的React组件有一个getDOMNode()方法，你可以调用这个方法来获取对该节点的引用。

为了获取一个到React组件的引用，你可以使用this来得到当前的React组件，或者你可以使用refs来指向一个你拥有的组件。
