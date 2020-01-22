---

title: Base Concept in React

date: 2020-01-16 10:19:32

tags: [React]

---

# React-Router

Guide line in Chinese:

[http://react-guide.github.io/react-router-cn/docs/guides/advanced/ConfirmingNavigation.html](http://react-guide.github.io/react-router-cn/docs/guides/advanced/ConfirmingNavigation.html)

[https://cn.redux.js.org/docs/advanced/UsageWithReactRouter.html](https://cn.redux.js.org/docs/advanced/UsageWithReactRouter.html) (use Redux with ReactRouter)

  

Guide line in English

[https://reacttraining.com/react-router/web/guides/quick-start](https://reacttraining.com/react-router/web/guides/quick-start)

[https://redux.js.org/advanced/usage-with-react-router/](https://redux.js.org/advanced/usage-with-react-router/) (use Redux with ReactRouter)

  

# Redux

## Action, Reducer,Store

[https://cn.redux.js.org/docs/introduction/](https://cn.redux.js.org/docs/introduction/) （Chinese）  

[https://redux.js.org/basics/basic-tutorial](https://redux.js.org/basics/basic-tutorial) （English）

  

Action: do some async job, like: ajax request, all the backend api. and then trigger a event to reducer

Reducer: listen to the event, and change the data in Store

Store: data store in it, and the components use `connect + mapStageToProps` to watch/compute the data change.

<!-- more -->

## different between Action and Reducer

[https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/65](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/65) （Chinese）

  

Connect + mapStagetoProps

[https://cn.redux.js.org/docs/basics/UsageWithReact.html](https://cn.redux.js.org/docs/basics/UsageWithReact.html)（Chinese）

[https://redux.js.org/basics/usage-with-react](https://redux.js.org/basics/usage-with-react)（English）