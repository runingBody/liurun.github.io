---
title: '高阶React: 初识State Hooks和Suspence'
tags:
  - react
originContent: >+
  钩子是即将推出的功能，它允许您在不编写类的情况下使用状态和其他React功能。他们目前在React v16.8.0-alpha.0。

  ::: hljs-center


  状态挂钩

  State Hooks


  :::

  我们将通过将此代码与等效的类示例进行比较来开始学习Hooks。

  例子钩：

  ```language

  import { useState } from 'react';


  function Example() {
    // Declare a new state variable, which we'll call "count"
    const [count, setCount] = useState(0);

    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
          Click me
        </button>
      </div>
    );
  }

  ```

  我们将通过将此代码与等效的类示例进行比较来开始学习Hooks。


  ### 等效类示例

  如果您之前在React中使用过类，那么这段代码看起来应该很熟悉：

  ```language

  class Example extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        count: 0
      };
    }

    render() {
      return (
        <div>
          <p>You clicked {this.state.count} times</p>
          <button onClick={() => this.setState({ count: this.state.count + 1 })}>
            Click me
          </button>
        </div>
      );
    }
  }

  ```

  状态开始为{ count: 0 }，state.count当用户通过调用单击按钮时，我们会递增this.setState()。


  ### 钩子和功能组件


  提醒一下，React中的函数组件如下所示：


  ```language

  const Example = (props) => {
    // You can use Hooks here!
    return <div />;
  }

  ```

  或这个：

  ```language

  function Example(props) {
    // You can use Hooks here!
    return <div />;
  }

  ```

  您可能以前将这些称为“无状态组件”。我们现在介绍从这些中使用React状态的能力，所以我们更喜欢名称“function components”。


  钩不工作的内部类。但是你可以使用它们而不是编写类。


  ### 介绍hooks

  我们的新示例首先useState从React 导入Hook：


  ```language

  import { useState } from 'react';


  function Example() {
    // ...
  }

  ```

  什么是钩子？Hook是一种特殊功能，可让您“挂钩”React功能。例如，useState是一个Hook，它允许您将React状态添加到功能组件。我们稍后会学习其他的Hook。


  我什么时候使用hook？如果你编写一个函数组件并意识到你需要为它添加一些状态，那么之前你必须将它转换为一个类。现在，您可以在现有功能组件中使用Hook。我们现在要做到这一点！


  ### 声明状态变量

  在类中，我们通过在构造函数中设置为初始化count状态：0this.state{ count: 0 }

  ```language

  class Example extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        count: 0
      };
    }
  ```

  在函数组件中，我们没有this，所以我们无法分配或读取this.state。相反，我们useState直接在组件内部调用Hook：

  ```language

  import { useState } from 'react';


  function Example() {
    // Declare a new state variable, which we'll call "count"
    const [count, setCount] = useState(0);
  ```

  打电话useState做什么？它声明了一个“状态变量”。调用我们的变量，count但我们可以将其称为其他任何东西，例如banana。这是一种在函数调用之间“保留”某些值的方法
  - useState是一种使用this.state类中提供的完全相同功能的新方法。通常，当函数退出时变量“消失”但React保留状态变量。


  我们useState作为一个论点传递给我们什么？useState()Hook
  的唯一参数是初始状态。与类不同，状态不必是对象。如果我们需要的话，我们可以保留一个数字或一个字符串。在我们的示例中，我们只需要一个数字来表示用户点击的次数，因此将0变量作为初始状态传递。（如果我们想在状态中存储两个不同的值，我们会调用useState()两次。）


  什么useState回报？它返回一对值：当前状态和更新它的函数。这就是我们写作的原因const [count, setCount] =
  useState()。这类似于this.state.count和this.setState在类中，除了你把它们成对。如果您不熟悉我们使用的语法，我们将在本页底部回到它。


  现在我们知道了useStateHook的作用，我们的例子应该更有意义：

  ```language

  import { useState } from 'react';


  function Example() {
    // Declare a new state variable, which we'll call "count"
    const [count, setCount] = useState(0);
  ```

  我们声明一个名为的状态变量count，并将其设置为0。React将记住重新渲染之间的当前值，并为我们的函数提供最新的值。如果我们想要更新当前count，我们可以打电话setCount。

  ```language

  注意


  您可能想知道：为什么useState不命名createState？


  “创建”不会非常准确，因为状态仅在我们的组件第一次呈现时创建。在下一次渲染期间，useState为我们提供当前状态。否则它根本不会是“状态”！Hook名称总是以一开始也是有原因的use。我们将在后来的“钩子规则”中了解原因。

  ```

  ### 阅读状态

  当我们想要在类中显示当前计数时，我们读到this.state.count：

  ```language
   <p>You clicked {this.state.count} times</p>
  ```

  在函数中，我们可以count直接使用：

  ```language
   <p>You clicked {count} times</p>
  ```

  ### 更新状态

  在类中，我们需要调用this.setState()以更新count状态：

  ```language

  <button onClick={() => this.setState({ count: this.state.count + 1 })}>
      Click me
    </button>
  ```


  在函数中，我们已经拥有了setCount与count作为变量，所以我们并不需要this：


  ```language
   <button onClick={() => setCount(count + 1)}>
      Click me
    </button>
  ```

  ### 概括

  现在让我们回顾一下我们逐行学习的内容并检查我们的理解。

  ```language
       import { useState } from 'react';
   
     function Example() {
       const [count, setCount] = useState(0);

       return (
         <div>
           <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
          Click me
          </button>
        </div>
     );
    }
  ```

  - 第1行：我们useState从React 导入Hook。它允许我们将本地状态保存在功能组件中。

  - 第4行：在Example组件内部，我们通过调用useStateHook
  声明一个新的状态变量。它返回一对值，我们给它们命名。我们调用变量count是因为它保存了按钮点击次数。我们通过传递0作为唯一useState参数将其初始化为零。第二个返回的项本身就是一个函数。它让我们更新，count所以我们将其命名setCount。

  - 第9行：当用户点击时，我们setCount使用新值调用。然后React将重新渲染Example组件，并将新count值传递给它。


  挂钩是向后兼容的。此页面提供了有经验的React用户的Hook概述。


  ### 钩子规则


  钩子是JavaScript函数，但它们强加了两个额外的规则：


  - 只能在顶层调用Hooks 。不要在循环，条件或嵌套函数中调用Hook。

  - 仅从React功能组件调用Hooks 。不要从常规JavaScript函数调用Hook。（还有另一个有效的地方叫Hooks -
  你自己的定制Hooks。我们马上就会了解它们。）


  ### 效果钩


  您之前可能已经从React组件执行数据提取，订阅或手动更改DOM。我们将这些操作称为“副作用”（或简称为“效果”），因为它们会影响其他组件，并且在渲染过程中无法完成。


  Effect Hook
  useEffect增加了从功能组件执行副作用的功能。它作为同样的目的componentDidMount，componentDidUpdate以及componentWillUnmount在阵营类，但统一到一个单一的API。（我们将useEffect在使用效果钩中显示与这些方法相比较的示例。）


  例如，此组件在React更新DOM后设置文档标题：

  ```language

  import { useState, useEffect } from 'react';


  function Example() {
    const [count, setCount] = useState(0);

    // Similar to componentDidMount and componentDidUpdate:
    useEffect(() => {
      // Update the document title using the browser API
      document.title = `You clicked ${count} times`;
    });

    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
          Click me
        </button>
      </div>
    );
  }

  ```


  当你打电话时useEffect，你告诉React在刷新对DOM的更改后运行你的“效果”功能。效果在组件内声明，因此可以访问其props和state。默认情况下，React在每次渲染后运行效果
  - 包括第一次渲染。（我们将更多地讨论这与使用效果钩中的类生命周期的比较。）


  效果还可以通过返回函数指定如何“清理”它们。例如，此组件使用效果来订阅朋友的在线状态，并通过取消订阅来清理：

  ```language

  import { useState, useEffect } from 'react';


  function FriendStatus(props) {
    const [isOnline, setIsOnline] = useState(null);

    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    useEffect(() => {
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    });

    if (isOnline === null) {
      return 'Loading...';
    }
    return isOnline ? 'Online' : 'Offline';
  }

  ```

  就像使用一样useState，您可以在组件中使用多个效果：

  ```language

  function FriendStatusWithCounter(props) {
    const [count, setCount] = useState(0);
    useEffect(() => {
      document.title = `You clicked ${count} times`;
    });

    const [isOnline, setIsOnline] = useState(null);
    useEffect(() => {
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    });

    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    // ...
  ```


  ::: hljs-center


  Sespence

  悬念


  :::


  持续跟新中...
















categories:
  - 前端
toc: false
date: 2019-01-18 15:23:22
---

钩子是即将推出的功能，它允许您在不编写类的情况下使用状态和其他React功能。他们目前在React v16.8.0-alpha.0。
::: hljs-center

状态挂钩
State Hooks

:::
我们将通过将此代码与等效的类示例进行比较来开始学习Hooks。
例子钩：
```language
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
我们将通过将此代码与等效的类示例进行比较来开始学习Hooks。

### 等效类示例
如果您之前在React中使用过类，那么这段代码看起来应该很熟悉：
```language
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
状态开始为{ count: 0 }，state.count当用户通过调用单击按钮时，我们会递增this.setState()。

### 钩子和功能组件

提醒一下，React中的函数组件如下所示：

```language
const Example = (props) => {
  // You can use Hooks here!
  return <div />;
}
```
或这个：
```language
function Example(props) {
  // You can use Hooks here!
  return <div />;
}
```
您可能以前将这些称为“无状态组件”。我们现在介绍从这些中使用React状态的能力，所以我们更喜欢名称“function components”。

钩不工作的内部类。但是你可以使用它们而不是编写类。

### 介绍hooks
我们的新示例首先useState从React 导入Hook：

```language
import { useState } from 'react';

function Example() {
  // ...
}
```
什么是钩子？Hook是一种特殊功能，可让您“挂钩”React功能。例如，useState是一个Hook，它允许您将React状态添加到功能组件。我们稍后会学习其他的Hook。

我什么时候使用hook？如果你编写一个函数组件并意识到你需要为它添加一些状态，那么之前你必须将它转换为一个类。现在，您可以在现有功能组件中使用Hook。我们现在要做到这一点！

### 声明状态变量
在类中，我们通过在构造函数中设置为初始化count状态：0this.state{ count: 0 }
```language
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```
在函数组件中，我们没有this，所以我们无法分配或读取this.state。相反，我们useState直接在组件内部调用Hook：
```language
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
```
打电话useState做什么？它声明了一个“状态变量”。调用我们的变量，count但我们可以将其称为其他任何东西，例如banana。这是一种在函数调用之间“保留”某些值的方法 - useState是一种使用this.state类中提供的完全相同功能的新方法。通常，当函数退出时变量“消失”但React保留状态变量。

我们useState作为一个论点传递给我们什么？useState()Hook 的唯一参数是初始状态。与类不同，状态不必是对象。如果我们需要的话，我们可以保留一个数字或一个字符串。在我们的示例中，我们只需要一个数字来表示用户点击的次数，因此将0变量作为初始状态传递。（如果我们想在状态中存储两个不同的值，我们会调用useState()两次。）

什么useState回报？它返回一对值：当前状态和更新它的函数。这就是我们写作的原因const [count, setCount] = useState()。这类似于this.state.count和this.setState在类中，除了你把它们成对。如果您不熟悉我们使用的语法，我们将在本页底部回到它。

现在我们知道了useStateHook的作用，我们的例子应该更有意义：
```language
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
```
我们声明一个名为的状态变量count，并将其设置为0。React将记住重新渲染之间的当前值，并为我们的函数提供最新的值。如果我们想要更新当前count，我们可以打电话setCount。
```language
注意

您可能想知道：为什么useState不命名createState？

“创建”不会非常准确，因为状态仅在我们的组件第一次呈现时创建。在下一次渲染期间，useState为我们提供当前状态。否则它根本不会是“状态”！Hook名称总是以一开始也是有原因的use。我们将在后来的“钩子规则”中了解原因。
```
### 阅读状态
当我们想要在类中显示当前计数时，我们读到this.state.count：
```language
 <p>You clicked {this.state.count} times</p>
```
在函数中，我们可以count直接使用：
```language
 <p>You clicked {count} times</p>
```
### 更新状态
在类中，我们需要调用this.setState()以更新count状态：
```language
<button onClick={() => this.setState({ count: this.state.count + 1 })}>
    Click me
  </button>
```

在函数中，我们已经拥有了setCount与count作为变量，所以我们并不需要this：

```language
 <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```
### 概括
现在让我们回顾一下我们逐行学习的内容并检查我们的理解。
```language
     import { useState } from 'react';
 
   function Example() {
     const [count, setCount] = useState(0);

     return (
       <div>
         <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
        Click me
        </button>
      </div>
   );
  }
```
- 第1行：我们useState从React 导入Hook。它允许我们将本地状态保存在功能组件中。
- 第4行：在Example组件内部，我们通过调用useStateHook 声明一个新的状态变量。它返回一对值，我们给它们命名。我们调用变量count是因为它保存了按钮点击次数。我们通过传递0作为唯一useState参数将其初始化为零。第二个返回的项本身就是一个函数。它让我们更新，count所以我们将其命名setCount。
- 第9行：当用户点击时，我们setCount使用新值调用。然后React将重新渲染Example组件，并将新count值传递给它。

挂钩是向后兼容的。此页面提供了有经验的React用户的Hook概述。

### 钩子规则

钩子是JavaScript函数，但它们强加了两个额外的规则：

- 只能在顶层调用Hooks 。不要在循环，条件或嵌套函数中调用Hook。
- 仅从React功能组件调用Hooks 。不要从常规JavaScript函数调用Hook。（还有另一个有效的地方叫Hooks - 你自己的定制Hooks。我们马上就会了解它们。）

### 效果钩

您之前可能已经从React组件执行数据提取，订阅或手动更改DOM。我们将这些操作称为“副作用”（或简称为“效果”），因为它们会影响其他组件，并且在渲染过程中无法完成。

Effect Hook useEffect增加了从功能组件执行副作用的功能。它作为同样的目的componentDidMount，componentDidUpdate以及componentWillUnmount在阵营类，但统一到一个单一的API。（我们将useEffect在使用效果钩中显示与这些方法相比较的示例。）

例如，此组件在React更新DOM后设置文档标题：
```language
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

当你打电话时useEffect，你告诉React在刷新对DOM的更改后运行你的“效果”功能。效果在组件内声明，因此可以访问其props和state。默认情况下，React在每次渲染后运行效果 - 包括第一次渲染。（我们将更多地讨论这与使用效果钩中的类生命周期的比较。）

效果还可以通过返回函数指定如何“清理”它们。例如，此组件使用效果来订阅朋友的在线状态，并通过取消订阅来清理：
```language
import { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```
就像使用一样useState，您可以在组件中使用多个效果：
```language
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // ...
```

::: hljs-center

Sespence
悬念

:::

持续跟新中...
















