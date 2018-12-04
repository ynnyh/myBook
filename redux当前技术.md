---
title: redux当前技术
tags: redux
---


### Prior Art 当前技术
Redux是一个混合产物，它同一些技术和模式相似，但是在一些重要的点上也有不同之处。我们将会探索其中的一些相似和不同之处。

#### Flux
Redux的由来受到了Flux的一些很重要的特质的启发，类似Flux。Redux规定你应该压缩你的model更新逻辑到你app的核心层中（“stores” in Flux, “reducers” in Redux）。而不是让你的应用代码直接更改数据，这些都告诉你把每一次的数据变动包装成一个叫做acton的对象。

不像Flux，Redux没有Dispatcher的概念。因为它依赖于纯函数来替代发送事件，并且纯函数是易于编写的还不用额外的实体来管理它们。这取决于你怎么看Flux，你可以将这点看做是两个框架的差异或者是细节实现，Flux经常被描述为 *(state, action) => state* 从这个意义上来说，Redux和Flux架构是相同的，但是得益于纯函数使它更简单了。

另一个和Flux不同的地方在于Redux假定你从来没有改动过你的数据。你可以为你的state使用对象和数组，这是相当赞的，但是强烈不建议在reducer中改变它们。你应该总是返回一个新对象，它是简单的使用 *object spread operator* 或者使用类似 *Immutable* 的库。

当然为了实现某些极端情况的场景，从技术上来说实现编写不纯的函数来改变数据是可行的，我们强烈不建议你这样做，因为这样会使开发特性比如时间旅行，日志记录/bug重现，或者热加载都将会被打断。此外，在一些真实的app中，这种数据不变性似乎不会导致性能问题，因为，就像 *[Om](https://github.com/omcljs/om)* 演示的那样，即使你对象分配失败，你仍然可以通过避免高昂的重渲染和重计算，因为得益于reducer的纯度，你可以准确的知道是什么发生了改变。

因为它的价值，Flux的创作者是支持Redux的。

#### Elm
Elm是一门函数式编程语言，由Evan Czaplicki受Haskell启发而创作的，它执行一种 *[model view update” architecture](https://github.com/evancz/elm-architecture-tutorial/)* 的架构，它的更新模式是(action, state) => state，Elm的“updaters”和Redux里的reducers服务于相同的目的。

不同于Redux，Elm是一门语言，所以它在很多事情上有优势比如执行纯度，静态类型，不可变动性和模式匹配。即使你不计划使用Elm，你也应该读一读这个架构并且尝试一下。有一个有趣的项目[使用JS库实现类似的想法](https://github.com/paldepind/noname-functional-frontend-framework) 。我们应该能从这儿找到更多的灵感在Redux上。有一个让我们可以更接近Elm的静态类型的方法是[使用类似Flow的渐进类型解决方案](https://github.com/reduxjs/redux/issues/290) 。

#### Immutable
Immutable是一个可以实现持久数据结构的JS库，它性能很好，并且有符合JS API命名习惯的API。

Immutable和许多跟其类似的库可以很好的和Redux对接使用，你尽可以放松的使用它们。

Redux不关心你怎么存储你的state，它可以是一个普通的对象，一个不可变对象或者其它的任何类型。你可能需要序列化（反序列化）机制来书写通用的app和从服务端将state更新并保持，但是除此之外，你可以使用任意的数据存储库只要它支持不可变动性。比如，对于Redux的state而言，使用Backbone毫无意义，因为Backbone的模式（models）是可变的。

注意，即使你的不可变库支持cursors，你也不应该在一个Redux app中使用它们。整个state树应该且只能是可读的，你应该使用Redux来更新这些state，并且订阅这些更新。因此编写cursor对于Redux而言毫无意义。如果你只是想用cursors来把state树从UI树解耦并细化cursors，你应该关注selectors。selectors是可组合的getter函数组。参阅[reselect](http://github.com/faassen/reselect) ，它是一个真正的优秀，简洁的可组合selector的实现。

#### Baobab
Baobab是另一个著名的库，为更新纯JS对象实现数据不可变特性的API。你可以把它和Redux一起使用，但是把它们一起使用几乎没有任何好处。

Baobab提供的大多数功能都和cursors更新数据有关，但是Redux坚持只有一中方法去更新数据也就是发起一次action（dispatch an action）。因此他们用另一种方法解决了同样的问题，而且彼此之间并没有互补之处。

不像Immutable，Baobab 在引擎下还没有实现任何高效的数据结构，所以你把它和Redux一起使用时，没有任何的好处，在这种情况下使用纯对象反而会更加简单。

#### RxJS
RxJS是一种非常高效的方案，用来管理异步应用程序的复杂性。实际上，[there is an effort to create a library that models human-computer interaction as interdependent observables](http://cycle.js.org/) 。

同时使用Redux和RxJS有意义吗？当然！使用它们一起工作会更好。比如，将Redux store视作一个可观察变量是容易的。

```
function toObservable(store) {
  return {
    subscribe({ next }) {
      const unsubscribe = store.subscribe(() => next(store.getState()))
      next(store.getState())
      return { unsubscribe }
    }
  }
}
```
同样地，你可以组合不同的异步流并转化为action，然后把它们提交给store.dispatch()。

问题是：你真的需要Redux吗即使你已经使用了Rx，或许不是的，在Rx中重新实现Redux不是很难的，有人说只需要写两行Rx的.scan()方法，这个可能不错。

如果你在疑惑，可以查看Redux的源代码（并不多）和它的生态系统。如果你不关心太多，并且坚持使用交互数据流，你可能想探索一些东西比如Cycle，甚至将它和Redux组合在一起，让我们知道它工作的情况。