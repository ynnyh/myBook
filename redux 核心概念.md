---
layout: post
title: redux 核心概念
tags: redux
---


### Core Concepts 核心概念
想象一下你的app的state被描述为一个对象（plain object），例如，一个todo app的state看起来可能像下面的这个例子：

```
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```

这个对象（object）像一个“model”除了没有setters。这就是代码的不同部分不能任意的修改state，从而导致出现难以复现的bug。

想要改变这个state中的某些东西，你需要dispatch an action。一个action是一个JS对象（plain JavaScript object）（注意我们没介绍任何魔法）用来描述发生了什么，下面是一些例子action：

```
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

强制使每一个改变都被描述为一个action这样会使我们对app的每一步运作有一个清晰的了解。任何东西的改动，我们会知道它为什么改动。action就像发生了什么改动的面包屑。最后，为了绑定state和action在一起，我们写了一个函数叫做reducer。再次强调，没有任何魔术在里面，它真的只是个函数，一个能够把state和action作为参数的函数（宝宝真的只是个函数），并且返回app的下一个state（即函数执行完毕返回新state）。为一个app写这样一个函数无疑是困难的，所以我们写了一个更小的函数来管理state的一部分。如下：

```
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map(
        (todo, index) =>
          action.index === index
            ? { text: todo.text, completed: !todo.completed }
            : todo
      )
    default:
      return state
  }
}
```

并且我们写了另一个reducer来管理我们的app的全部state通过为对应的state键调用那两个reducer（by calling those two reducers for the corresponding state keys）。如下：

```
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```

这基本上是Redux的核心思想（This is basically the whole idea of Redux）。注意我们还没使用任何关于Redux的API。它附带了一些工具来促进这个模式，但是主要的想法是你怎么描述你的state的更新在response中随着时间的流逝给action objects（把你怎么在得到response后更新你的state到action objects中），而且你写的代码的90%是JS，并没有使用到Redux自己或者其他什么魔术。
