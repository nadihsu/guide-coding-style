# Table of Contents
- [原則](#原則)
- [格式](#格式)
- [命名規則](#命名規則)
- [變數](#變數)
- [宣告的順序](#宣告的順序)
- [檔案結構](#檔案結構)
- [外部資源](#外部資源)
- [參考](#參考)

---

# 原則
- 不使用 ID 選擇器
原因：
  - ID 做的到的 class 都做的到
  - ID 不能重複使用
  - ID 有太高的權重
- compile 完的 css 盡可能不超過兩層深
  若需超過兩層，先檢查 DOM 的結構有沒有可以優化的地方
- 命名和歸檔，盡量做到想改什麼，就能馬上聯想到該檔案名稱
- 能簡寫就寫簡寫
  - 能寫 `margin: 0 40px 20px`，就不要寫四個宣告
  - 如果有特定需要，就先寫簡寫，再寫特定宣告（比如 `backgorund-size` 有時會需要）
- 重複性工作應用迴圈或寫成 mixin 處理

# 格式

- 一般狀況使用 sass，除非需要使用 map 或者多行的程式語法才用 scss
- 用 soft tabs（2 個空白，可參考 [sublime 設定](https://github.com/unfoldgroup/guide-dev-environment/tree/master/editor#preference)）
- 不使用 ID 選擇器
- 當需要使用多個選擇器時，一個選擇器打一行
- 宣告和宣告之間留空行
- 冒號後空一格
- 色碼全小寫，能寫三碼就不寫六碼

**bad**
```sass
.avatar
    border-radius:50%
    border:2px solid white
.no, .nope, .not_good
    // ...

#lol-no
    // ...

```

**good**
```sass
.avatar
  border-radius: 50%
  border: 2px solid white

.one,
.selector,
.per-line
  // ...
```

# 命名規則
全小寫命名，且使用類似 [BEM](http://getbem.com/introduction/) 的 class 命名方式，舉例：

**jade**
```jade
article.listing_card.listing_card-featured
  h1.listing_card__title Adorable 2BR in the sunny Mission
  .listing_card__content
    p Vestibulum id ligula porta felis euismod semper.
```

**sass**
```sass
.listing_card
  // ...
  &-featured
    // ...
  &__title
    // ...
  &__content
    // ...
```
### 說明
- `.listing_card` 代表高層元件 (Block)
- `.listing_card__title` 代表 block 下的元素 (Element)
- `.listing_card-feature` 代表變形 (Modifier)

### 注意
- Element 深度僅一層，也就是不會出現 `.block_name__element_A__element_B` 這樣的階層

## 前綴系統
- `.is-<status>`  
  代表元件狀態（和 `modifier` 有點類似，小心搞混）  
  舉例：按鈕的 `.btn.is-active` 是狀態，`.btn.btn-danger` 是變形
- `.has-<object>` 代表元件內有特定子元件
- `.js-<object name>` 和 js 功能有關係的，加一個 js 當前綴，提醒修改者要修改時須考慮 js
- `.page-<page name>` 只會用在 html tag 上，留給頁面特定的 style 使用

# 變數
一律使用英文小寫字母撰寫變數名稱，而變數名稱為兩個英文單字以上時，請使用 “ - ” 字號來區隔單字，例如：`$color-blue` `$background-color`... 以方便閱讀。
針對同系列的變數單字排列順序，盡可能讓順序越一致越好，舉個例子：
```sass
// bad
$color-light-blue: #???
$color-blue: #???
$color-dark-blue: #???

// good
$color-blue-light: #???
$color-blue: #???
$color-blue-dark: #???
```

# 宣告的順序
不強制規定，不過習慣上把 `@extend %placeholder` 擺最前面，`+mixin` 擺最後面，中間其他宣告的順序，依據 style 或 layout 分組，比如
```sass
// bad
.something
  @extend %something-else
  background: #000
  +pie-clearfix
  padding: 10px
  margin: 20px 0
  color: #fff
  display: block

// good 1: 把 box model 相關和純 style 分開歸納
.something
  @extend %something-else
  display: block
  margin: 20px 0
  padding: 10px

  background: #000
  color: #fff
  +pie-clearfix

// good 2: 把和接下來要做變化的宣告和其他宣告分開
.some-other-things
  background: #000
  transition: padding .3s, color .3s

  padding: 10px
  color: #fff
  &:hover
    padding: 15px
    color: #c00
```

# 檔案結構
```
.
├── all.sass
├── _layout.sass
├── _mixin.sass
├── _shared-config.scss
├── _typography.sass
├── _variable.sass
├── base
│   ├── _base.sass
│   ├── _blockquote.sass
│   ├── _code.sass
│   ├── _divider.sass
│   ├── _heading.sass
│   └── _list.sass
├── component
│   ├── _button.sass
│   ├── _component-name.sass
│   ├── _form.sass
│   └── _pagination.sass
├── page
│   ├── _page-name.sass
│   └── _rwd-image-sample.sass
├── partial
│   └── _partial-name.sass
└── vendor
    └── _typecsset.scss
```
- 檔案入口
  - `all.sass` 只有開頭不是底線的檔案會被編譯成最終的 css 檔，可依需求新增
- 以下為暫存檔，當該檔案內容多到應該新增資料夾時，可自行開新的資料夾加以整理
  - `_layout.sass` 放整體排版相關的 style，比如 grid system
  - `_mixin.sass` 放 mixin
  - `_typography.sass` 定義字體
  - `_variables.sass` 定義變數
- 與 jade 和 coffee 共用變數的設定檔
  - `_shared-config.scss` 此檔案為自動生成，主要是 rwd 的 breakpoints
- 各大資料夾
  - `base` 資料夾放純 tag 的 style，如 `a`, `p`, `ul`, `li`.. 等
  - `component` 放元件
  - `page` 放特定頁面 style
  - `partial` 放部件
  - `vendor` 放無法用 npm install 安裝的 dependency

# 外部資源
需要外部資源時，先看看 npm 上面有沒有，如果有的話安裝之後，寫進 gulp 的 compile:sass 裡。
目前有使用的功能：
- gulp-css-globbing: 一次 import 一個資料夾的 sass
- normalize.css
- sass-mq: media query 用
- autoprefixer: 自動加前綴
- postcss-assets: 處理 image url 的功能

如果 npm 上面沒有就下載下來放到 vendor 裡（如果數量多，且 bower 上有的話也可考慮使用 bower）

# 參考
- https://github.com/airbnb/css
- http://mvcss.ycnets.com/
