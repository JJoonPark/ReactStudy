# ReactStudy

### React란?
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
### Props를 통해 데이터 전달하기
&lt;Square value={i} />; 처럼 i를 Square Component에 전달하고, Square Component 내부에서는 this.props.value를 이용하여 i 값을 전달 받는다.

### 사용자와 상호작용하는 Component 만들기
button 태그의 onClick props에 `() => console.log('click')` 과 같이 화살표 함수나 function을 전달해야 한다. `onClick={console.log('click')}`과 같이 전달하면 React는 Component가 다시 렌더링 될 때마다 경고 창을 띄울 것이다.

Square Component를 클릭한 것을 "기억하게" 만들어 "X" 표시를 채워 넣으려면, 무언가를 "기억하기" 위해 Component는 **state**를 사용한다.

React Component는 생성자에 this.state를 설정하는 것으로 state를 가질 수 있다. this.state는 정의된 React Component에 대해 비공개로 간주해야 한다. 이제 Square의 현재 값을 this.state에 저장하고 Square를 클릭하는 경우 변경하도록 해보자.

```
class Square extends React.Component {
  constructor(props) {
  super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'}))}
      >
        {this.state.value}
      </button>
    );
  }
}
```
Square의 render 함수 내부에서 onClick 핸들러를 통해 this.setState를 호출하는 것으로 React에게 <button>을 클릭할 때 Square가 다시 렌더링해야 한다고 알릴 수 있다.
업데이트 이후에 Square의 this.state.value는 'X'가 되어 게임 판에서 X가 나타나는 것을 확인할 수 있다. 어떤 Square를 클릭하던 X가 나타날 것이다.
  
Component에서 setState를 호출하면 React는 자동으로 컴포넌트 내부의 자식 Component 역시 업데이트 한다.

### State 끌어올리기
게임의 state를 각각의 Square Component에서 유지하고 있다. 승자를 확인하기 위해 9개 사각형의 값을 한 곳에 유지해야 한다.
  
Board Component에 게임의 상태를 저장하는 것이 가장 좋은 방법이다. 각 Square에 숫자를 넘겨주었을 때와 같이 Board Component는 각 Square에게 prop을 전달하는 것으로 무엇을 표시할지 알려준다.
  
**여러개의 자식으로부터 데이터를 모으거나 두 개의 자식 Component들이 서로 통신하게 하려면 부모 Component에 공유 state를 정의해야 한다. 부모 Component는 props를 사용하여 자식 Component에 state를 다시 전달할 수 있다. 이것은 자식 Component들이 서로 또는 부모 Component와 동기화 하도록 한다**
  
Square를 클릭하면 Board에서 넘겨받은 onClick 함수가 호출된다. 이 때 일어나는 일을 정리해보자.
  1. 내장된 DOM &lt;button> Component에 있는 onClick prop은 React에게 클릭 이벤트 리스너를 설정하라고 알려준다.
  2. 버튼을 클릭하면 React는 Square의 render() 함수에 정의된 onClick 이벤트 핸들러를 호출한다.
  3. 이벤트 핸들러는 this.props.onClick()를 호출한다. Square의 onClick prop은 Board에서 정의되어 있다.
  4. Board에서 Square로 `onClick={() => this.handleClick(i)}`를 전달했기 때문에 Square를 클릭하면 Board의 handleClick(i)를 호출한다.
  
**DOM &lt;button> 엘리먼트의 onClick 어트리뷰트는 내장된 Component라는 점에서 React에게 특별한 의미가 있다. Square같은 사용자 정의 Component의 경우 이름 지정은 자유롭다. Square의 onClick prop이나 Board의 handleClick 함수에는 어떤 이름도 붙일 수 있으며 코드는 동일하게 작동한다. React에서 이벤트를 나타내는 prop에는 on[Event], 이벤트를 처리하는 함수에는 handle[Event]를 사용하는 것이 일반적이다.**

### 불변성의 중요성
handleClick에서는 .slice()를 호출하는 것으로 기존 배열을 수정하지 않고 squares 배열의 복사본을 생성하여 수정하는 것의 주의해야 한다.
기존 배열을 수정하는 것이 아니라 .slice() 연산자를 사용하여 squares 배열의 사본 만들기를 추천했다. 일반적으로 데이터 변경에는 두 가지 방법이 있다. 첫 번째는 데이터의 값을 직접 변경하는 것이다. 두 번째는 원하는 변경 값을 가진 새로운 사본으로 데이터를 교체하는 것이다.
  1. 복잡한 특징들을 단순하게 만듦
    직접적인 데이터 변이를 피하는 것은 이전 버전의 게임 이력을 유지하고 나중에 재사용할 수 있게 만든다.
  2. 변화를 감지함.
    객체가 직접적으로 수정되기 떄문에 복제가 가능한 객체에서 변화를 감지하는 것은 어렵다. 감지는 복제가 가능한 객체를 이전 사본과 비교하고 전체 객체 트리를 돌아야 한다.
    불변 객체에서 변화를 감지하는 것은 상당히 쉽다. 참조하고 있는 불변 객체가 이전 객체와 다르다면 객체는 변한 것이다.
  3. React에서 다시 렌더링하는 시기를 결정함.
    불변성의 가장 큰 장점은 React에서 순수 Component를 만드는 데 도움을 준다는 것이다. 변하지 않는 데이터는 변경이 이루어졋는지 쉽게 판단할 수 있으며 이를 바탕으로 Component가 
    다시 렌더링할지를 결정할 수 있다.
  
### 함수 Component
React에서 함수 Component는 더 간단하게 Component를 작성하는 방법이며 state없이 render 함수만을 가진다. React.Component를 확장하는 클래스를 정의하는 대신 props를 입력받아서 렌더링할 대상을 반환하는 함수를 작성할 수 있다. 함수 Component는 클래스로 작성하는 것보다 빠르게 작성할 수 있으며 많은 Component를 함수 Component로 표현할 수 있다.

```
  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
```
render와 return 사이에 함수를 실행하도록 해 놓으면 매 렌더링 마다 해당 함수를 호출할 수 있다.
  
```
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
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
결과를 비교해볼 lines를 정의하고 lines의 인자를 배열로 받아 비교하여 결과를 나타낼 수 있음.
                                   
```
handleClick(i) {
  const history = this.state.history;
  const current = history[history.length - 1];
  const squares = current.squares.slice();
  if (calculateWinner(squares) || squares[i]) {
    return;
  }
  squares[i] = this.state.xIsNext ? 'X' : 'O';
  this.setState({
    history: history.concat([{
      squares: squares,
    }]),
    xIsNext: !this.state.xIsNext,
  });
}                                 
```
** 배열 push() 함수와 같이 더 익숙한 방식과 달리 concat() 함수는 기존 배열을 변경하지 않기 때문에 더 권장된다. **
                                   
### Key 선택하기
리스트를 렌더링할 때 React는 렌더링하는 리스트 아이템에 대한 정보를 저장한다. 리스트를 업데이트 할 때 React는 무엇이 변했는지 결정해야 한다. 리스트의 아이템들은 추가, 제거, 재배열 업데이트 될 수 있다.
