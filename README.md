### ReactStudy

#React란?
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

ShoppingList 컴포넌트는 "<div />"와 "<li />" 같은 내각 DOM 컴포넌트만을 렌더링하지만 컴포넌트를 조합하여 커스텀 React 컴포넌트를 렌더링하는 것도 가능하다. 예를 들어 <ShoppingList />를 작성하여 모든 쇼핑 목록을 참조할 수 있다. React 컴포넌트는 캡슐화되어 독립적으로 동작할 수 있다. 이러한 이유로 단순한 컴포넌트를 사용하여 복잡한 UI를 구현할 수 있다.
