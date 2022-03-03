---
title: React Notes
date: 2021-12-26 9:00:00
tags:
- react
- javascript
header_image: /intro/index-bg.jpg
categories:
  - frontend
---
This post contains notes learned while setting up a tutorial React website.
<!-- more -->

Examples from [Tutorial: Intro to React](https://reactjs.org/tutorial/tutorial.html)

Next up
- [x] [Tutorial: Intro to React](https://reactjs.org/tutorial/tutorial.html)
- [ ] [hello-world](https://reactjs.org/docs/hello-world.html)
- [ ] [gatsbyjs](https://www.gatsbyjs.com/starters/)

## Terms
Term | Defination
--- | ---
props | properties

Variable | Scope | Updatable
--- | --- | ---
var | global | yes
let | block | yes
const | block | no

## Loop over Array
Useful to create tables and lists.
```
return <div>
    {user.map((person, index) => (
        <h1>Hello, {<Welcome name={formatName(person)} />}</h1>
    ))}
</div>
```
## Conditional Operator
```
condition ? true : false
```

## Function as property
```
onClick={() => console.log('click')}
```

## Functions
Simplier way to write compoents with only a render compoent and no state. Uses `props.x` format.
```
function Square(props) {
  return (
    <button className="square"
      onClick={props.onSquareClick}
    >
      {props.squareValue}
    </button>
  )
}
```

## Class
Uses `this.props.x` format.

```
class Board extends React.Component {
  render() {
    return (
      <div>
        {this.props.message}
        </div>
    );
  }
}
```

## Method (inside class)
```
  renderSquare(i) {
    return (
      <Square
        squareValue={this.props.squares[i]}
        onSquareClick={() => this.props.onSquareClick(i)}
      />
    );
  }
```

## Constructor (inside class)
Used to save state.
```
  constructor(props) {
    super(props)
    this.state = {
      stepNumber: 0,
      xIsNext: true,
    }
  }
```

## Use state inside event (inside class)
```
    constructor(props) {
        super(props);
        this.handleClick = this.handleClick.bind(this)
    }
        handleClick() {
            blah
    }
```
Or experimental
```
handleClick = () => {
    console.log('this is:', this);
  }
```

## Arguments to events
```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
