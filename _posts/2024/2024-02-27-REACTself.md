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

여기서 시작해서 아래 코드까지 하면 1부. 컴포넌트 구현하기 1 까지 끝난다

```1=App.jsx
import React from "react";
import Header from "./components/Header.js";
import SearchForm from "./components/SearchForm.js";
import SearchResult from "./components/SearchResult.js";
import store from "./Store.js";
import Tab, { TabType } from "./components/Tabs.js";
import Tabs from "./components/Tabs.js";

export default class App extends React.Component {
  constructor() {
    super();

    this.state = {
      searchKeyword: "",
      searchResult: [],
      submitted: false,
      selectedTab: TabType.KEYWORD,
    };
  }
  search(searchKeyword) {
    const searchResult = store.search(searchKeyword);
    this.setState({
      searchResult,
      submitted: true,
    });
  }
  handleReset() {
    this.setState({ searchKeyword: "", searchResult: [], submitted: false });
  }
  handleChangeInput(searchKeyword) {
    if (searchKeyword.length <= 0) {
      this.handleReset();
    }
    this.setState({ searchKeyword });
  }
  render() {
    const { searchKeyword, searchResult, submitted, selectedTab } = this.state;
    return (
      <>
        <Header title="검색" />
        <div className="container">
          <SearchForm
            value={searchKeyword}
            onSubmit={() => this.search(searchKeyword)}
            onReset={() => this.handleReset()}
            onChange={(value) => this.handleChangeInput(value)}
          />
        </div>
        <div className="content">
          {submitted ? (
            <SearchResult data={searchResult} />
          ) : (
            <>
              <Tabs
                selectedTab={selectedTab}
                onChange={(selectedTab) => this.setState({ selectedTab })}
              />
              {selectedTab === TabType.KEYWORD && <>TODO:추천검색</>}
              {selectedTab === TabType.HISTORY && <>TODO:최근검색</>}
            </>
          )}
        </div>
      </>
    );
  }
}

```

```2=SearchForm.js

import React from "react";

const SearchForm = ({ value, onChange, onSubmit, onReset }) => {
  const handleReset = () => {
    onReset();
  };
  const handleSubmit = (event) => {
    event.preventDefault();
    onSubmit();
  };
  const handleChangeInput = (event) => {
    onChange(event.target.value);
  };
  return (
    <form onReset={handleReset} onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="검색어를 입력하세요"
        autoFocus
        value={value}
        onChange={handleChangeInput}
      />
      {value.length > 0 && <button type="reset" className="btn-reset"></button>}
    </form>
  );
};
export default SearchForm;

```

```3=SearchResult.js

import React from "react";

const SearchResult = ({ data = [] }) => {
  if (data.length <= 0) {
    return <div className="empty-box">검색 결과가 없습니다</div>;
  }
  return (
    <ul className="result">
      {data.map(({ id, imageUrl, name }) => (
        <li key={id}>
          <img src={imageUrl} />
          <p>{name}</p>
        </li>
      ))}
    </ul>
  );
};

export default SearchResult;

```

```4=Tabs.js

import React from "react";
export const TabType = {
  KEYWORD: "KEYWORD",
  HISTORY: "HISTORY",
};

export const TabLabel = {
  [TabType.KEYWORD]: "추천 검색어",
  [TabType.HISTORY]: "최근 검색어",
};

const Tabs = ({ selectedTab, onChange }) => {
  return (
    <>
      <ul className="tabs">
        {Object.values(TabType).map((tabType) => (
          <li
            key={tabType}
            className={selectedTab === tabType ? "active" : ""}
            onClick={() => onChange(tabType)}
          >
            {TabLabel[tabType]}
          </li>
        ))}
      </ul>
    </>
  );
};

export default Tabs;


```
