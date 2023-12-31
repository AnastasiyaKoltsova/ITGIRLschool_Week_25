# ITGIRLschool_Week_25

## Теоретические вопросы
### 1. В компонент передаются атрибуты description и title. Могу ли я их сложить как на примере, чтобы получить одну строку и вывести в компоненте?
```
import React from "react";
import styles from './button.css'

export default class Example extends React.Component {
    render() {
				let {description, title} = this.props
				title += description; //title = title + description
         return (
            <span>{title}</span>
        );
    }
}
```
Да, можно складывать строки в React компоненте, используя оператор "+=" так же, как показано в примере. Оператор "+=" присоединяет значение переменной description к переменной title, создавая новую строку, и сохраняет ее в переменной title. 

### 2. С помощью какого метода можно отловить изменение props? 
В React для отслеживания изменений props можно использовать методы жизненного цикла компонента, такие как `componentDidUpdate` или `getDerivedStateFromProps`.

`componentDidUpdate`: этот метод вызывается после обновления компонента и позволяет выполнить какие-либо действия при изменении props. Внутри этого метода можно сравнивать предыдущие и текущие props и выполнять необходимые действия.

Пример:

```
componentDidUpdate(prevProps) {
  if (this.props.someProp !== prevProps.someProp) {
    // Выполнить действия при изменении свойства someProp
  }
}
```

`getDerivedStateFromProps`: этот статический метод вызывается перед рендерингом компонента и позволяет обновить состояние компонента на основе измененных props. Внутри этого метода можно возвращать новое состояние, которое будет установлено перед рендерингом.

Пример:

```
static getDerivedStateFromProps(nextProps, prevState) {
  if (nextProps.someProp !== prevState.someProp) {
    return { someState: nextProps.someProp };
  }
  return null;
}
```

Оба эти метода могут быть использованы для отслеживания изменения props и выполнения соответствующих действий в React компонентах.

### 3. Оператор расширения часто используется также для клонирования объекта. Подумайте, чем отличаются эти две записи и какую проблему решает здесь оператор расширения:
```
const initialObj = { title:'Hello', text:'World' }

//№1
const firstObj = initialObj

//№2
const secondObj = {...initialObj}
```
В первой записи переменная `firstObj` ссылается на тот же объект, на который ссылается `initialObj`. То есть, если изменить значение свойства `title` или `text` у `firstObj`, то значение этих свойств также изменится и у `initialObj`. Это происходит, потому что в JavaScript передача объектов осуществляется по ссылке.

Во второй записи переменная `secondObj` содержит новый объект, который является поверхностной копией объекта `initialObj`. При изменении значения свойства `title` или `text` у `secondObj` значения этих свойств в объекте `initialObj` не изменится. Оператор расширения `{...initialObj}` создает новый объект и копирует в него все свойства из `initialObj`.

Оператор расширения используется здесь для создания независимой копии объекта `initialObj`. Это решает проблему, связанную с передачей объектов по ссылке и возможностью нежелательного изменения оригинального объекта при изменении его копии.

### 4. В каком из методов жизненного цикла лучше всего использовать методы вызова API и обращения к веб-хранилищам, если они должны быть вызваны всего один раз при загрузке страницы?
Лучше всего использовать метод жизненного цикла компонента `componentDidMount`.

Метод `componentDidMount` вызывается сразу после того, как компонент был добавлен в DOM страницы. Именно здесь рекомендуется выполнять загрузку данных из API или обращение к веб-хранилищам, поскольку уже есть доступ к DOM элементам.

### 5. С помощью какого метода можно отловить и отрисовать для пользователя возникшую в компоненте ошибку?
Для отлова и отрисовки ошибки в компоненте можно использовать метод `componentDidCatch`. 

### 6. Какой код обычно пишут в конструкторе? Для каких задач он используется?

1. Инициализация состояния (state) компонента: В конструкторе компонента можно задать начальное состояние компонента, указав значение для this.state.

2. Привязка контекста: Если в компоненте используются собственные методы, которые будут вызываться в разных контекстах (например, обработчики событий), то в конструкторе можно привязать контекст к этим методам, чтобы они правильно ссылались на this компонента.

3. Привязка обработчиков событий: В конструкторе можно привязать обработчики событий к текущему компоненту, чтобы они имели доступ к его методам и состоянию.

4. Выполнение инициализаций компонента: Если необходимо выполнить какие-либо дополнительные действия при инициализации компонента, такие как загрузка данных или подписка на события, то это можно сделать в конструкторе.

5. Определение начальных свойств (props): В конструкторе можно определить начальные значения для свойств компонента, которые будут переданы при создании экземпляра компонента.

### 7. Что делает функция render()? Что может её вызвать?
Функция render() в React отвечает за отрисовку компонента или элемента в виртуальном DOM (virtual DOM). Она принимает два аргумента: компонент или элемент, который нужно отрисовать, и контейнер, в котором производится отрисовка.

Render() может быть вызвана двумя способами:

1. Из корневого компонента с помощью ReactDOM.render(). Этот метод используется при первоначальной отрисовке приложения и обновлении DOM при изменении состояния. Например:
```
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```

2. Внутри компонента. При изменении состояния компонента, его render() метод вызывается автоматически, что приводит к перерисовке компонента и его дочерних компонентов, если не был применен механизм shouldComponentUpdate(), который позволяет оптимизировать перерисовку. Например:
```
class ExampleComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  handleClick() {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={() => this.handleClick()}>Increment</button>
      </div>
    );
  }
}
```
В данном примере каждый раз, когда пользователь кликает на кнопку, метод handleClick() вызывается, изменяет состояние компонента и вызывает перерисовку с помощью render().

### 8. Что нужно изменить в коде из урока (видео), чтобы начальные параметры у компонента приходили из пропсов, но если пропсы вообще не заданы, начальные значения были инициализированы нулями?
- добавить запись:
 static defaultProps = {
    minutes: 0,
    seconds: 0
 }

### 9. Можно ли несколько раз использовать хук useEffect внутри одного компонента?
Да, можно помещать несколько хуков useEffect внутрь одного компонента. Каждый хук useEffect будет выполняться независимо друг от друга. 

### 10. Можно ли не передавать второй аргумент в хук useEffect? Что тогда произойдёт?
Второй аргумент в хуке useEffect представляет собой массив зависимостей. При изменении значений в этом массиве, запускается колбэк-функция, переданная в первом аргументе. 

Если второй аргумент не передан, то колбэк-функция будет запускаться после каждого рендера компонента, что может привести к нежелательному поведению, такому как повторное выполнение запросов к серверу. 

Для избежания таких проблем, рекомендуется всегда указывать зависимости во втором аргументе хука useEffect.

### 11. Что означает возвращение функции в теле хука `useEffect`? 
Возвращение функции в теле хука useEffect имеет специальное значение. 

Когда компонент, в котором используется хук useEffect, отмонтируется или компонент обновится, перед запуском нового эффекта будет выполнена функция, возвращенная из предыдущего эффекта. Это позволяет очистить ресурсы или отменить подписки, которые были созданы в предыдущем эффекте.

Например, если внутри хука useEffect создан таймер или выполнена подписка на событие, то возвращение функции из хука useEffect позволит остановить таймер или отписаться от событий при размонтировании или обновлении компонента. Это предотвращает утечку памяти и другие проблемы, связанные с неуправляемыми ресурсами.

### 12. Будут ли перерисованы дочерние элементы компонента при вызове метода `forceUpdate`()?
Да, при вызове метода `forceUpdate()` будут перерисованы все дочерние элементы компонента. Метод `forceUpdate()` вызывает обновление компонента, что приводит к перерисовке его дочерних элементов независимо от их текущего состояния или пропсов. 

## Практическое задание № 1
Попробуйте переписать классовый компонент Counter, используя хуки:

```
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(prevCount => prevCount + 1);
  };

  return (
    <button onClick={handleClick}>
      {count}
    </button>
  );
}

export default Counter;
```