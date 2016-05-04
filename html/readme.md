[starter2016](https://github.com/unfoldgroup/starter2016) 用 jade 當 template engine，語法見[官網](http://jade-lang.com/reference/)

# Table of Contents
- [原則](#原則)
- [格式](#格式)
- [檔案結構](#檔案結構)
- [Helper](#Helper)

---

# 原則
- 最後交付檔案時盡量把資料和樣板分離
- 重複的 code 用迴圈或者 mixin 處理

# 格式

## 使用變數時，等號後面加一空白：
```jade
//- bad
p=text

//- good
p= text
```

## 使用變數時，可視情況使用 interpolation
```jade
//- bad
p= 'copyright ©' + current_year

//- good
p copyright ©#{current_year}
```

## 註解
```jade
// 會輸出
//- 不會輸出
```

## 屬性間不加逗號
```jade
//- bad
a(href='#', target='_blank')

//- good
a(href='#' target='_blank')
```

## 資料分離
為方便日後維護或者將原始檔交付給後端接資料，資料和樣板最好分離（但不是硬性規定，可視專案規模決定）

**bad**
```jade
//- _menu.jade
ul
  li:a(href='/about') about
  li:a(href='/product') product
  li:a(href='/blog') blog
  li:a(href='/contact') contact
```

**good 1**
```jade
//- _menu.jade
-
  var links = [
    { text: 'about', href: '/about' },
    { text: 'product', href: '/product' },
    { text: 'blog', href: '/blog' },
    { text: 'contact', href: '/contact' }
  ]

ul
  each link in links
    li:a(href=link.href)= link.text
```

**good 2**
```coffee
# data/nav.coffee
module.exports = [
    text: 'about'
    href: '/about'
  ,
    text: 'product'
    href: '/product'
  ,
    text: 'blog'
    href: '/blog'
  ,
    text: 'contact'
    href: '/contact'
  ]
```
```jade
//- _menu.jade
links = readData('nav')
ul
  each link in links
    li:a(href=link.href)= link.text
```


# 檔案結構
```
.
├── data
│   ├── global.coffee
│   └── info.coffee
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

# Helper
目前 jade helper 寫在 [gulp libs](https://github.com/unfoldgroup/starter2016/blob/master/gulp%2Flibs%2Fcompile%2Fjade.coffee) 裡，可依專案需求調整。
