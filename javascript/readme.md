[starter2016](https://github.com/unfoldgroup/starter2016) 用 coffee 當 js preprocessor ，語法見[官網](http://coffeescript.org/)

# Table of Contents
- [原則](#原則)
- [格式](#格式)
- [空白](#空白)
- [命名慣例](#命名慣例)
- [模組](#模組)
- [函數](#函數)
- [字串](#字串)
- [DOM 操作](#dom 操作)
- [檔案結構](#檔案結構)
- [參考](#參考)

---
# 原則
- 以可讀性和效能為重，不用為了把 code 寫短，而排斥抽出變數
- 一個檔案一般的編輯順序是
  1. 先載入相依模組
  - 定義變數
  - 定義函數
  - 做 dom 和函數的事件綁定
  - 進行初始化或輸出模組（如果有必要）

# 格式
## 省略逗號
```coffee
# bad
foo = [
  'some',
  'string',
  'values'
]

bar =
  label: 'test',
  value: 87

# good
foo = [
  'some'
  'string'
  'values'
]

bar =
  label: 'test'
  value: 87
  
# good 2: 夠簡短到覺得一行也沒關係的狀況
foo = ['some', 'string', 'values']
bar = { label: 'test', value: 87 }
```

## 空白
- 前後一空白
  - 賦值：`=`, `+=`, `-=`, `||=` 等
  - 比較：`==`, `<`, `>`, `<=`, `>=`, `unless` 等
  - 運算：`+`, `-`, `*`, `/`, `^` 等
  - 邏輯：`&&`, `||`
  - function: `->`
- 後一空白
  - 冒號 `:`
  - 逗號 `,`
  - 註解 `#`
- 括號
  - 花括號 `{  }` 內左右各一空白
  - 方括號 `[]` 內不留空白
  - 圓括號 `()` 內不留空白

```coffee
#bad
a=17
b={key:'value'}
c=[ a,b ]
d = (val)->doSomething( a+val )
d() if a&&b

# good
a = 17
b = { key: 'value' }
c = [a, b]
d = (val) -> doSomething(a + val)
d() if a && b
```
## this
使用 `@` 不打 `this`，`@` 後面不接 `.`
```coffee
this.getOptions() # bad
@.getOptions() # bad
@getOptions() # good
```

# 命名慣例
- 使用 camelCase 命名變數、函數、方法、物件屬性等
- 使用 PascalCase 命名 class
- 用全大寫和底線代表常數（唯讀值）：`CONSTANT_LIKE_THIS`
- class 內私有方法用底線開頭：`_privateMethod`
- 使用成對單字：
  - `add` / `remove`
  - `create` / `delete`
  - `show` / `hide`
  - `get` / `set`
  - `next` / `prev`
  - ...
- 變數名稱用名詞：`userAge`, `user.age` ..
- 函數或方法名稱使用動詞：`getUserAge()`, `user.getAge()`, `user.setAge(28)` ..
- 函數回傳值是布林時，用 `is` 或 `has` 開頭：`isLogin()`
- jQuery object 用 `$` 開頭：`$users = $('.js-user')`

# 模組
- 一個 `require` 一行

  ```coffee
  require 'lib/setup'
  Backbone = require 'backbone'
  ```
- require 後路徑的意思：
  - 載入透過 npm 安裝於 node_modules 裡的 package：`require 'jquery'` 
  - 載入自己寫的 package，加 `./` 代表目前檔案目錄位置：`require './something'`
- 用解構賦值（Destructuring Assignment）載入
  ```coffee
  # bad
  funcitonA = require('/module-name').functionA
  funcitonB = require('/module-name').functionB
  
  # bad 2
  module = require('/module')
  funcitonA = module.functionA
  funcitonB = module.functionB  
  
  # good
  { functionA, functionB } = require('/module')
  ```
- 用解構賦值（Destructuring assignment）輸出
  ```coffee
  # bad
  someThing = 1
  someThingOther = 2
 
  module.exports = 
    someThing: someThing
    someThingOther: someThingOther
    
  # good
  someThing = 1
  someThingOther = 2
 
  module.exports = { someThing, someThingOther }
  ```

# 函數
當沒有參數需要傳入時，省略圓括號

```coffee
a = () -> # bad
b = -> # good
```
呼叫函數時，在程式不特別複雜的狀況下可以省略圓括號
```coffee
say('hi')
say 'hi'

$ '.js-something'
  .addClass 'is-active'
  .appendTo 'body'
```
連鎖（chaining）時，可以用 indent 來增加易讀性
```coffee
$('.js-item-list')
  .find('.is-selected')
    .highlight()
    .end()
  .find('.is-disabled')
    .remove()
```

# 字串
平常宣告字串時，都用單引號 `'say hi'`
需要插入變數時，需用雙引號 `"say hi to #{currentUser}"`

# DOM 操作
- 要直接被操作的 dom 要記得加上以 `js-` 為前綴的 class name
- 重複操作的 jquery object 要 cache 起來，避免每次操作同一個 dom 就執行一次 jquery function：  
  `$selectors = $('.selector-name')`

# 檔案結構
```
.
├── global.coffee
├── page-name.coffee
└── util
    ├── _util-name.js
    └── _xx.coffee
```
- 和 sass 一樣，僅有非底線開頭的檔案會被 compile 成 js 檔。global.coffee 代表全站都會 import 的 js，page-name.coffee 為特定頁面使用之 js（需要在 jade 自行引入），如果有 ie 需求也可以寫一個 ie.coffee
- util 是放各種小工具的地方

# 參考
- https://github.com/polarmobile/coffeescript-style-guide
