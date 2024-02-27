---
layout: post
title: 리액트 복습
categories: 공부중
tags: [taeyeon]
---

## 1부 복습!

혼자 연습하는 중!

다음번에도 연습하려면 이 상황 만들어놓고 시작하면 된다

```1=App.js

import React from "react";
import Header from "./components/Header.js";
import SearchForm from "./components/SearchForm.js";

export default class App extends React.Component {
  render() {
    return (
      <>
        <Header title="검색" />
        <div className="container">
          <SearchForm />
        </div>
      </>
    );
  }
}

```

```2=SearchForm.js
class SearchForm extends React.Component {
  //   const handleSubmit
  //   const handleReset
  //   const handleChangeInput
  render() {
    return (
      <form>
        <input type="text" placeholder="검색어를 입력하세요" autoFocus />
        <button type="reset" className="btn-reset"></button>
      </form>
    );
  }
}
```
