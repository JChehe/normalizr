# normalizr [![build status](https://img.shields.io/travis/paularmstrong/normalizr/master.svg?style=flat-square)](https://travis-ci.org/paularmstrong/normalizr) [![Coverage Status](https://img.shields.io/coveralls/paularmstrong/normalizr/master.svg?style=flat-square)](https://coveralls.io/github/paularmstrong/normalizr?branch=master) [![npm version](https://img.shields.io/npm/v/normalizr.svg?style=flat-square)](https://www.npmjs.com/package/normalizr) [![npm downloads](https://img.shields.io/npm/dm/normalizr.svg?style=flat-square)](https://www.npmjs.com/package/normalizr)

## Motivation
## 动机

Many APIs, public or not, return JSON data that has deeply nested objects. Using data in this kind of structure is often [very difficult](https://groups.google.com/forum/#!topic/reactjs/jbh50-GJxpg) for JavaScript applications, especially those using [Flux](http://facebook.github.io/flux/) or [Redux](http://redux.js.org/).

大部分 API（无论是否公开），返回的 JSON 数据都含有深层的嵌套对象。对于 JavaScript 应用（特别是使用 Flux 或 Redux），使用该类型数据时常常会感到困难。

## Solution
## 解决方案

Normalizr is a small, but powerful utility for taking JSON with a schema definition and returning nested entities with their IDs, gathered in dictionaries.

Normalizr 是一个轻量却强大的工具，它通过定义 schema 将 JSON 转换为带有自身 ID 的 嵌套实体，汇集在字典中。

## Documentation
## 文档

* [Introduction](/docs/introduction.md)
* [介绍](/docs/introduction.md)
* [Quick Start](/docs/quickstart.md)
* [快速开始](/docs/quickstart.md)
* [API](/docs/api.md)
* [API](/docs/api.md)
    - [normalize](/docs/api.md#normalizedata-schema)
    - [规范化](/docs/api.md#normalizedata-schema)
    - [denormalize](/docs/api.md#denormalizeinput-schema-entities)
    - [反规范化](/docs/api.md#denormalizeinput-schema-entities)
    - [schema](/docs/api.md#schema)
    - [模式](/docs/api.md#schema)
* [Using with JSONAPI](/docs/jsonapi.md)
* [结合 JSONAPI 使用](/docs/jsonapi.md)

## Examples
## 案例

* [Normalizing GitHub Issues](/examples/github)
* [Relational Data](/examples/relationships)
* [Interactive Redux](/examples/redux)

## Quick Start
## 快速开始

Consider a typical blog post. The API response for a single post might look something like this:  

考虑一个典型的博客帖子。API 返回一个帖子可能是以下数据：

```json
{
  "id": "123",
  "author": {
    "id": "1",
    "name": "Paul"
  },
  "title": "My awesome blog post",
  "comments": [
    {
      "id": "324",
      "commenter": {
        "id": "2",
        "name": "Nicole"
      }
    }
  ]
}
```

We have two nested entity types within our `article`: `users` and `comments`. Using various `schema`, we can normalize all three entity types down:  

我们的 `article` 内有两个嵌套的实体：`users` 和 `comments`。使用各自 `schema`，我们能规范化（`normalize`）前面提到的三个实体类型：

```js
import { normalize, schema } from 'normalizr';

// Define a users schema
// 定义一个 users schema
const user = new schema.Entity('users');

// Define your comments schema
// 定义你的 comments schema
const comment = new schema.Entity('comments', {
  commenter: user
});

// Define your article 
// 定义你的 article
const article = new schema.Entity('articles', { 
  author: user,
  comments: [ comment ]
});

const normalizedData = normalize(originalData, article);
```

Now, `normalizedData` will be:  

现在，`normalizedData` 变量将是：

```js
{
  result: "123",
  entities: {
    "articles": { 
      "123": { 
        id: "123",
        author: "1",
        title: "My awesome blog post",
        comments: [ "324" ]
      }
    },
    "users": {
      "1": { "id": "1", "name": "Paul" },
      "2": { "id": "2", "name": "Nicole" }
    },
    "comments": {
      "324": { id: "324", "commenter": "2" }
    }
  }
}
```

## Dependencies
## 依赖项

None.  

无。

## Credits
## 归功于

Normalizr was originally created by [Dan Abramov](http://github.com/gaearon) and inspired by a conversation with [Jing Chen](https://twitter.com/jingc). Since v3, it was completely rewritten and maintained by [Paul Armstrong](https://twitter.com/paularmstrong). It has also received much help, enthusiasm, and contributions from [community members](https://github.com/paularmstrong/normalizr/graphs/contributors).  

Normalizr 最初由 [Dan Abramov](http://github.com/gaearon) 创建，这源于他与 [Jing Chen](https://twitter.com/jingc) 的对话中受到了启发。从 v3 开始，Normalizr 被 [Paul Armstrong](https://twitter.com/paularmstrong) 完全重写并维护。它也从 [社区成员](https://github.com/paularmstrong/normalizr/graphs/contributors) 中得到很多的帮助、鼓励与贡献。
