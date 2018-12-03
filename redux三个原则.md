---
layout: post
title: redux三个原则 
tags: redux
---


### Three Principles（三个原则）

Redux可以用三个基本原则来描述：

#### Single source of truth（单一的数据源）
你的app的全部state被存储在一个对象树的单一store中。
它使创建一个app变得容易，因为来自服务器的state可以和客服端无缝对接。一个单一的state树可以使debug容易许多或者检查一个app容易很多；它还可以使你在开发中保持你的app的state，对于一个更快的开发周期而言，一些传统的很难实现的功能例如撤销/重做，可以变得轻而易举，如果你把你的全部state存储在一个单一的树里。

```
console.log(store.getState())

/* Prints
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
*/
```

#### State is read-only （state是只读的）
唯一能修改state的方法是发出一个action，一个对象描述发生了什么。

这就确保了无论是视图还是网络请求的回调都不能直接修改state，取代的是，它们表达一种转换state的意图。因为所有的改变都被集中起来并且通过队列一个接一个进行，没有特别需要注意的情况。因为action只是一个对象，它们可以被日志记录，序列化，存储而且后期debug（调试）和测试可以重现。

```
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

#### Changes are made with pure functions （使用纯函数改变）
为了指定state树怎么更改通过action，你需要书写纯reducer

reducer是纯函数携带有一个state和一个action，并且返回更新过后的state，切记函数要返回新的state对象。你可以用一个单独的reducer开始，随着你的app的壮大，你可以把它切分成小的reducer用来管理state树的具体的一小部分。因为reducer仅仅是一个函数，你可以控制它们被调用的顺序，传入附加数据，甚至编写可复用的reducer来处理通用事务例如分页器。

```
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}

import { combineReducers, createStore } from 'redux'
const reducer = combineReducers({ visibilityFilter, todos })
const store = createStore(reducer)
```

是的，现在你知道redux的全部了。