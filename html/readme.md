# Table of Contents
- [Jade](#jade)
- [原則](#原則)
- [格式](#格式)
- [檔案結構](#檔案結構)
- [Helper](#Helper)

---

# Jade
[starter2016](https://github.com/unfoldgroup/starter2016) 用 jade 當 template engine，語法見[官網](http://jade-lang.com/reference/)

# 原則
- 最後交付檔案時盡量把資料和樣板分離
- 重複的 code 用迴圈或者 mixin 處理

# 格式
- 使用變數時，等號後面加一空白：`p= text`
- 使用 `// ` 做顯示註解，`//- ` 做隱藏註解

**bad**
```jade
// _menu.jade
ul
  li:a(href='/about') about
  li:a(href='/product') product
  li:a(href='/blog') blog
  li:a(href='/contact') contact
```

**good**
```yml
# data.yml
links:
  -
    text: 'about'
    href: '/about'
  - 
    text: 'product'
    href: '/product'
  -
    text: 'blog'
    href: '/blog'
  - 
    text: 'contact'
    href: '/contact'
```
```jade
// _menu.jade
ul
  each link in links
    li:a(href=link.href)= link.text
```


# 檔案結構
```
.
├── data
│   ├── global.yml
│   ├── index.yml
│   └── page-name.yml
└── jade
    ├── layouts
    │   ├── _default.jade
    │   └── shared
    ├── mixin
    │   └── _picture.jade
    ├── pages
    │   ├── about
    │   ├── index.jade
    │   ├── page-name.jade
    │   └── rwd-image-sample.jade
    └── partials
        ├── _code.jade
        ├── _footer.jade
        ├── _form-demo.jade
        ├── _header.jade
        └── _partial-name.jade
```
jade 的 data source 來自 data 資料夾，首先會先往 global.yml 找，然後再去找相對的頁面，key 重複時則以頁面變數為主。案子不複雜的狀況可以都寫在 global.yml 裡，頁面資料量大需要分檔時再分檔即可。

# Helper
目前 jade helper 寫在 gulp task 裡，可依專案需求調整。
