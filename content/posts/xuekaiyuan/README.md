---
title: README
date: 2023-01-19
publishdate: 2023-01-19
---

<!-- markdownlint-disable-next-line MD025 -->
# 学开源

## 目录

- [学开源](#学开源)
    - [目录](#目录)
    - [简介](#简介)
    - [版本](#版本)
        - [`v0.1`](#v01)
        - [`v0.2`](#v02)
        - [`v0.3`](#v03)
    - [仓库结构](#仓库结构)
    - [`git submodule`（子模块）](#git-submodule子模块)
    - [GitHub Action](#github-action)
    - [`Hugo`配置](#hugo配置)
    - [`/README.md`](#readmemd)
    - [原型（内容模板）](#原型内容模板)
    - [内容](#内容)
        - [`/content/posts/_index.md`](#contentposts_indexmd)
        - [`/content/posts/xuekaiyuan/README.md`](#contentpostsxuekaiyuanreadmemd)
    - [模板](#模板)
        - [`/layouts/_default/baseof.html`](#layouts_defaultbaseofhtml)
        - [`/layouts/_default/list.html`](#layouts_defaultlisthtml)
        - [`/layouts/posts/single.html`](#layoutspostssinglehtml)

## 简介

本仓库为<https://www.xuekaiyuan.com/>源代码，仓库在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io>

## 版本

### `v0.1`

初始版本，基于

<https://github.com/huzhenghui/Hugo-Bootstrap-CSS>

位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1>

### `v0.2`

修改为符合<https://www.xuekaiyuan.com/>设置

位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.2>

### `v0.3`

增加<https://github.com/huzhenghui/posts/>为`git submodule`（子模块），
并借助`GitHub Action`在<https://github.com/huzhenghui/posts/>更新时自动更新，
详见<https://www.xuekaiyuan.com/huzhenghui/github/readme/>

位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.3>

## 仓库结构

命令

```shell
tree -a -F -I '.git/'
```

结果

```text
./
├── .github/
│   └── workflows/
│       └── hugo.yml
├── .gitignore
├── .gitmodules
├── .hugo_build.lock
├── README.md -> ./content/xuekaiyuan/README.md
├── archetypes/
│   └── default.md
├── config.toml
├── content/
│   ├── huzhenghui/
│   │   ├── .git
│   │   ├── .github/
│   │   │   └── workflows/
│   │   │       └── main.yml
│   │   ├── GitHub/
│   │   │   └── README.md
│   │   ├── README.md -> ./GitHub/README.md
│   │   └── _index.md
│   └── posts/
│       ├── Hugo-Bootstrap-CSS/
│       │   └── README.md
│       ├── _index.md
│       └── xuekaiyuan/
│           └── README.md
├── layouts/
│   └── _default/
│       ├── baseof.html
│       ├── list.html
│       └── single.html
└── resources/
    └── _gen/
        ├── assets/
        └── images/

17 directories, 18 files
```

## `git submodule`（子模块）

在<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/>中，
运行如下命令将<https://github.com/huzhenghui/posts/>添加为子模块

```shell
git submodule add https://github.com/huzhenghui/posts content/huzhenghui
```

命令中<https://github.com/huzhenghui/posts>为子模块仓库的位置，
`content/huzhenghui`为添加的位置，添加后可以看到`git submodule`（子模块）的配置文件

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/.gitmodules>

```toml
[submodule "content/huzhenghui"]
 path = content/huzhenghui
 url = https://github.com/huzhenghui/posts
```

子模块添加在<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/master/content>，
其中可以看到`huzhenghui`，单击即可进入<https://github.com/huzhenghui/posts/>

## GitHub Action

基于`GitHub`自动创建的`Hugo` `Action`，位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/.github/workflows/hugo.yml>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)
使用`GitHub`自动创建的`Hugo` `Action`

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.1/.github/workflows/hugo.yml>

[v0.3](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.3)
响应<https://github.com/huzhenghui/posts/>中`GitHub Action`触发的事件，并增加`git submodule`（子模块）自动更新。

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.3/.github/workflows/hugo.yml>

其中在`on`中增加响应`repository_dispatch`事件，
`repository_dispatch`事件在[`huzhenghui/posts`的`GitHub Action`文件](https://www.xuekaiyuan.com/huzhenghui/github/readme/#huzhenghuiposts%E7%9A%84github-action%E6%96%87%E4%BB%B6)中触发。

```diff
 on:
   # Runs on pushes targeting the default branch
   push:
     branches: ["master"]
 
   # Allows you to run this workflow manually from the Actions tab
   workflow_dispatch:
+  repository_dispatch:
```

在`jobs`的`Checkout`之后`Setup Pages`之前增加`Update module`

```diff
       - name: Checkout
         uses: actions/checkout@v3
         with:
           submodules: recursive
+      - name: Update module
+        run: git submodule update --remote
       - name: Setup Pages
         id: pages
         uses: actions/configure-pages@v2
```

## `Hugo`配置

`Hugo`的配置文件，位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/config.toml>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)

[v0.2](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.2)
修改为符合<https://www.xuekaiyuan.com/>设置

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.2/config.toml>

```toml
baseURL      = 'https://www.xuekaiyuan.com/'
languageCode = 'zh-cn'
title        = '学开源'
```

## `/README.md`

软链接，链接到[`content/posts/xuekaiyuan/README.md`](#contentpostsxuekaiyuanreadmemd)

Since [v0.2](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.2)

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.2/README.md>

## 原型（内容模板）

`Hugo`自动创建的文件，位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/archetypes/default.md>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.1/archetypes/default.md>

```markdown
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
---
```

## 内容

### `/content/posts/_index.md`

索引页，内容显示在

<https://www.xuekaiyuan.com/posts/>

文件位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/content/posts/_index.md>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)

[v0.2](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.2)
修改为符合<https://www.xuekaiyuan.com/>设置

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.2/content/posts/_index.md>

```markdown
---
title: 学开源 Posts（索引页标题）
date: 2023-01-19
publishdate: 2023-01-19
---

<!-- markdownlint-disable-next-line MD025 -->
# 学开源 Posts

（索引页正文内容）
```

### `/content/posts/xuekaiyuan/README.md`

本文件，位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/content/posts/xuekaiyuan/README.md>

Since [v0.2](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.2)

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.2/content/posts/xuekaiyuan/README.md>

## 模板

### `/layouts/_default/baseof.html`

基本模板，位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/layouts/_default/baseof.html>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.1/layouts/_default/baseof.html>

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{{ block "title" . }}
        {{ .Site.Title }}
        {{ end }}
    </title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
</head>

<body class="container">
    {{ block "main" . }}
    {{ end }}
    {{ block "footer" . }}
    {{ end }}
</body>

</html>
```

### `/layouts/_default/list.html`

列表页面模版，效果参见

<https://www.xuekaiyuan.com/posts/>

文件位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/layouts/_default/list.html>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.1/layouts/_default/list.html>

```html
{{ define "main" }}
<main>
    <article>
        <header>
            <h1 class="display-1">{{.Title}}</h1>
        </header>
        {{.Content}}
    </article>
    <ul class="list-group list-group-flush">
        {{ range .Pages }}
        <li class="list-group-item">
            <a class="link-primary" href="{{.Permalink}}">{{.Date.Format "2006-01-02"}} | {{.Title}}</a>
        </li>
        {{ end }}
    </ul>
</main>
{{ end }}
```

### `/layouts/posts/single.html`

单独页面模板，效果参见

<https://www.xuekaiyuan.com/posts/xuekaiyuan/readme/>

文件位置在

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/master/layouts/_default/single.html>

Since [v0.1](https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/tree/v0.1)

<https://github.com/xuekaiyuan-com/xuekaiyuan-com.github.io/blob/v0.1/layouts/_default/single.html>

```html
{{ define "main" }}
<section id="main">
    <h1 id="title" class="display-1">{{ .Title }}</h1>
    <div>
        <article id="content" class="lead">
            {{ .Content }}
        </article>
    </div>
</section>
<aside id="meta">
    <div>
        <section>
            <h4 id="date"> {{ .Date.Format "Mon Jan 2, 2006" }} </h4>
            <h5 id="wordcount"> {{ .WordCount }} Words </h5>
        </section>
        {{ with .GetTerms "topics" }}
        <ul id="topics">
            {{ range . }}
            <li><a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a></li>
            {{ end }}
        </ul>
        {{ end }}
        {{ with .GetTerms "tags" }}
        <ul id="tags">
            {{ range . }}
            <li><a href="{{ .RelPermalink }}">{{ .LinkTitle }}</a></li>
            {{ end }}
        </ul>
        {{ end }}
    </div>
    <div>
        {{ with .PrevInSection }}
        <a class="previous link-primary" href="{{.Permalink}}"> {{.Title}}</a>
        {{ end }}
        {{ with .NextInSection }}
        <a class="next link-primary" href="{{.Permalink}}"> {{.Title}}</a>
        {{ end }}
    </div>
</aside>
{{ end }}
```
