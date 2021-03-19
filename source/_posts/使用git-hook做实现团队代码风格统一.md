---
title: 使用git hook做实现团队代码风格统一
date: 2021-03-19 19:50:41
tags: [git, 项目工程化]
---

使用 Git Hook 实现多人开发时，实现代码风格统一，以及代码校验等。

<!--more-->

## 实现流程

Husky 可以在 commit 和 push 之前执行自定义的操作，如使用 prettier 和 elint 检查和自动格式化代码, line-staged 可以针对暂存的 git 文件运行多个指令

## 安装依赖

```
npm i -D husky lint-staged eslint eslint-config-airbnb prettie
```

## 添加.eslintrc.js 文件

```
    //cd > | touch .eslintrc.js

    module.exports = {
    extends: ['airbnb'],
    rules: {
        'react/jsx-filename-extension': 'off',
        //You can override any rules you want
    },
    };
```

## 添加.prettierrc.js 文件

具体配置请参考 prettier 文档

```
    odule.exports = {
    bracketSpacing: true,
    jsxBracketSameLine: true,
    singleQuote: true,
    trailingComma: 'all',
    // Override any other rules you want
    };
```

//添加相关配置到 package.json

```
    "husky": {
        "hooks": {
            "pre-commit": "lint-staged"，
            “pre-push”: "npm run test"
        }
    },
    "lint-staged": {
        "./src/**/*.{js,jsx,ts,tsx}": [
            "npx prettier --write", // npx 可以省略
            "eslint src/*.js --fix-dry-run",
        ]
    }
```
