### ReactStudy

##React란?
React는 사용자 인터페이스를 구착하기 위한 선언적이고 효율적이며 유연한 javaScript 라이브러리.
"Component"라고 불리는 작고 고립된 코드의 파편을 이용하여 복잡한 UI를 구성하도록 한다.

```
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
```
ShoppingList는 **React 컴포넌트 클래스** 또는 **React 컴포넌트 타입**이라고 부른다. 개별 컴포넌트는 props라는 매개변수를 받아오고 render 함수를 통해 표시할 뷰 계층 구조를 반환 함.
render 함수는 화면에서 보고자 하는 내용을 반환. render는 렌더링할 내용을 경량화한 React 엘리먼트를 반환. React 개발자는 "JSX"라는 특수한 문법을 사용해서 React의 구조를 쉽게 작성할 수 있다. <div /> 구문은 빌드하는 시점에서 React.createElement('div')로 변환된다. 위에서 본 예시는 아래와 같이 변화한다.
```
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

JSX는 JavaScript의 강력한 기능을 가지고 있다. JSX 내부의 중괄호 안에 어떤 JavaScript 표현식도 사용할 수 있다. React 엘리먼트는 JavaScript 객체이며 변수에 저장하거나 프로그램 여기저기에 전달할 수 있다.

ShoppingList 컴포넌트는 &lt;div />와 &lt;li /> 같은 내각 DOM 컴포넌트만을 렌더링하지만 컴포넌트를 조합하여 커스텀 React 컴포넌트를 렌더링하는 것도 가능하다. 예를 들어 <ShoppingList />를 작성하여 모든 쇼핑 목록을 참조할 수 있다. React 컴포넌트는 캡슐화되어 독립적으로 동작할 수 있다. 이러한 이유로 단순한 컴포넌트를 사용하여 복잡한 UI를 구현할 수 있다.

```
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}

class Board extends React.Component {
  renderSquare(i) {
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [
        {
          squares: Array(9).fill(null)
        }
      ],
      stepNumber: 0,
      xIsNext: true
    };
  }

  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? "X" : "O";
    this.setState({
      history: history.concat([
        {
          squares: squares
        }
      ]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext
    });
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0
    });
  }

  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = "Winner: " + winner;
    } else {
      status = "Next player: " + (this.state.xIsNext ? "X" : "O");
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={i => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(<Game />, document.getElementById("root"));

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```
##Props를 통해 데이터 전달하기
&lt;Square value={i} />; 처럼 i를 Square Component에 전달하고, Square Component 내부에서는 this.props.value를 이용하여 i 값을 전달 받는다.

##사용자와 상호작용하는 Component 만들기
button 태그의 onClick props에 `() => console.log('click')` 과 같이 화살표 함수나 function을 전달해야 한다. `onClick={console.log('click')}`과 같이 전달하면 React는 Component가 다시 렌더링 될 때마다 경고 창을 띄울 것이다.

