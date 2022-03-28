### ReactStudy

#React란?
React는 사용자 인터페이스를 구착하기 위한 선언적이고 효율적이며 유연한 javaScript 라이브러리.
"Component"라고 불리는 작고 고립된 코드의 파편을 이용하여 복잡한 UI를 구성하도록 한다.

`class ShoppingList extends React.Component {
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
}`
