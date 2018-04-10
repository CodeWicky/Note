# Weex学习笔记

Weex整体上与Vue语法大概一直，基本用法由阿里进行二次封装。以下主要介绍Weex的一些内置组件。

--- 

### \<a\>
与HTML中行为基本一直，作为超链接进行页面跳转。

```
<a href="http://dotwe.org/raw/dist/a5e3760925ac3b9d68a3aa0cc0298857.bundle.wx">
  <text>Jump</text>
</a> 
```
---

### \<div\>

与HTML中行为基本一直，作为包装其他组件最基本的容器。

> 关于\<div\>的层级嵌套不宜过深，官方建议控制在10层以内，避免造成性能影响。

```
<template>
  <div>
    <div class="box"></div>
  </div>
</template>

<style scoped>
  .box {
    border-width: 2px;
    border-style: solid;
    border-color: #BBB;
    width: 250px;
    height: 250px;
    margin-top: 250px;
    margin-left: 250px;
    background-color: #EEE;
  }
</style>
```

---

### \<image\>

图片标签


> 注意：在HTML中通常使用的 \<img\> 标签在 Weex 中不支持，你应该使用\<image\> 。

> 注意： Weex 没有内置的图片下载器，因为相关的下载、缓存、解码机制非常复杂，一些开源的工具如 SDWebImage 已经处理得很好， 所以在使用 \<image\> 之前，请在 native 侧先接入相应的 adapter 或者 handler。

> 参见: Android adapter 和 iOS handler。

```
<image style="width:500px;height:500px" src="https://vuejs.org/images/logo.png"></image>
```

#### 属性

|属性名|类型|值|默认值|
|:---:|:---:|:---:|:---:|
|placeholder|String|{URL/Base64}|-|
|resize|String|cover/contain/stretch|stretch|
|src|String|{URL/Base64}|必填|
|style|String|width:500px;height:500px|必填|

#### save

需要设备赋予权限，v0.16.0版本后支持。

参数：

- callback：{Function} 在图片被写入到本地文件或相册后的回调，回调参数：
	- result：{Object} 回调结果对象，属性列表：
		- success：{Boolean} 标记图片是否已写入完成。
		- errorDesc：{String} 如果图像没有成功写入，该字符串包含了详细的错误描述。

函数调用示例：

```
<image ref="poster" src="path/to/image.png"></image>

const $image = this.$refs.poster
$image.save(result => {
  if (result.success) {
    // Do something to hanlde success
  } else {
    console.log(result.errorDesc)
    // Do something to hanlde failure
  }
})
```

#### load

支持获取图片加载完成回调，通过v-on监听事件。

示例代码：

```
<image @load="onImageLoad" src="path/to/image.png"></image>

export default {
  methods: {
    onImageLoad (event) {
      if (event.success) {
        // Do something to hanlde success
      }
    }
  }
}
```

#### 使用说明

- 在使用 \<image\> 之前，请在 native 侧先接入相应的 adapter 或者 handler。
- \<image\> 必须指定样式中的宽度和高度。
- \<image\> 不支持内嵌子组件。

---
### \<indicator\>

指示器，暂时无用，后期补充资料。

---
### \<input\>

与HTML中行为进本一致。以多种形式接收用户输入。

> 此组件不支持 click 事件。请监听 input 或 change 来代替 click 事件。

> 次组件不支持子组件。

#### 特性

- type {string}：控件的类型，默认值是 <text>。type 值可以是 text，date，datetime，email， password，tel，time，url，number 。每个 type 值都符合 W3C 标准。
- value {string}：组件的默认内容。
- placeholder {string}：提示用户可以输入什么。 提示文本不能有回车或换行。
- disabled {boolean}：布尔类型的数据，表示是否支持输入。通常 click 事件在 disabled 控件上是失效的。
- autofocus {boolean}：布尔类型的数据，表示是否在页面加载时控件自动获得输入焦点。
- maxlength {nubmer}：v0.7一个数值类型的值，表示输入的最大长度。
- return-key-type {string}：v0.11键盘返回键的类型,支持 defalut;go;next;search;send,done。
- singleline {boolean}：控制内容是否只允许单行
- max-length {number}：控制输入内容的最大长度
- lines：控制输入内容的最大行数
- max：控制当type属性为date时选择日期的最大时间，格式为yyyy-MM-dd
- min：控制当type属性为date时选择日期的最小时间，格式为yyyy-MM-dd

#### 事件

- input: 输入字符的值更改。

	事件中 event 对象属性：
	
	- value: 触发事件的组件；
	- timestamp: 事件发生时的时间戳,仅支持Android。
		
- change: 当用户输入完成时触发。通常在 blur 事件之后。

	事件中 event 对象属性：

	- value: 触发事件的组件；
	- timestamp: 事件发生时的时间戳,仅支持Android。

- focus: 组件获得输入焦点。

	事件中 event 对象属性：

	- timestamp: 事件发生时的时间戳,仅支持Android。
- blur: 组件失去输入焦点。

	事件中 event 对象属性：

	- timestamp: 事件发生时的时间戳,仅支持Android。
- return: 键盘点击返回键。

	事件中 event 对象属性：

	- returnKeyType: 事件发生时的返回键类型。
	- value: 触发事件的组件的文本；

- 通用事件

	> 不支持 click 事件。 请监听 input 或 change 事件代替。

	支持以下通用事件：

	- longpress
	- appear
	- disappear

示例代码

```
<template>
  <div>
    <div>
      <text style="font-size: 40px">oninput: {{txtInput}}</text>
      <text style="font-size: 40px">onchange: {{txtChange}}</text>
      <text style="font-size: 40px">onreturntype: {{txtReturnType}}</text>
      <text style="font-size: 40px">selection: {{txtSelection}}</text>

    </div>
    <scroller>
      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = text</text>
        </div>
        <input type="text" placeholder="Input Text" class="input" :autofocus=true value="" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = password</text>
        </div>
        <input type="password" placeholder="Input Password" class="input" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = url</text>
        </div>
        <input type="url" placeholder="Input URL" class="input" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = email</text>
        </div>
        <input type="email" placeholder="Input Email" class="input" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = tel</text>
        </div>
        <input type="tel" placeholder="Input Tel" class="input" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = time</text>
        </div>
        <input type="time" placeholder="Input Time" class="input" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = number</text>
        </div>
        <input type="number" placeholder="Input number" class="input" @change="onchange" @input="oninput"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input type = date</text>
        </div>
        <input type="date" placeholder="Input Date" class="input" @change="onchange" @input="oninput" max="2017-12-12" min="2015-01-01"/>
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input return-key-type = default</text>
        </div>
        <input type="text" placeholder="please input" return-key-type="default" class="input" @change="onchange" @return = "onreturn" @input="oninput" />
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input return-key-type = go</text>
        </div>
        <input type="text" placeholder="please input" return-key-type="go" class="input" @change="onchange" @return = "onreturn" @input="oninput" />
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input return-key-type = next</text>
        </div>
        <input type="text" placeholder="please input" return-key-type="next" class="input" @change="onchange" @return = "onreturn" @input="oninput" />
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input return-key-type = search</text>
        </div>
        <input type="text" placeholder="please input" return-key-type="search" class="input" @change="onchange" @return = "onreturn" @input="oninput" />
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input return-key-type = send</text>
        </div>
        <input type="text" placeholder="please input" return-key-type="send" class="input" @change="onchange" @return = "onreturn" @input="oninput" />
      </div>

      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input return-key-type = done</text>
        </div>
        <input type="text" placeholder="please input" return-key-type="done" class="input" @change="onchange" @return = "onreturn" @input="oninput" />
      </div>


      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">function focus() & blur()</text>
        </div>
        <div style="flex-direction: row;margin-bottom: 16px;justify-content: space-between">
          <text class="button" value="Focus" type="primary" @click="focus"></text>
          <text class="button" value="Blur" type="primary" @click="blur"></text>
        </div>

        <input type="text" placeholder="Input1" class="input" value="" ref="input1"/>
      </div>


      <div>
        <div style="background-color: #286090">
          <text class="title" style="height: 80 ;padding: 20;color: #FFFFFF">input selection</text>
        </div>
        <div style="flex-direction: row;margin-bottom: 16px;justify-content: space-between">
          <text class="button" value="setRange" type="primary" @click="setRange"></text>
          <text class="button" value="getSelectionRange" type="primary" @click="getSelectionRange"></text>
        </div>
        <input type="text"  ref="inputselection" placeholder="please input" value="123456789"  class="input" @change="onchange" @return = "onreturn" @input="oninput"/>
      </div>



    </scroller>
  </div>
</template>

<style scoped>
  .input {
    font-size: 60px;
    height: 80px;
    width: 750px;
  }
  .button {
    font-size: 36;
    width: 200;
    color: #41B883;
    text-align: center;
    padding-top: 10;
    padding-bottom: 10;
    border-width: 2;
    border-style: solid;
    margin-right: 20;
    border-color: rgb(162, 217, 192);
    background-color: rgba(162, 217, 192, 0.2);
  }
</style>

<script>
  module.exports = {
    data: function () {
      return {
        txtInput: '',
        txtChange: '',
        txtReturnType: '',
        txtSelection:'',
        autofocus: false
      };
    },
    methods: {
      ready: function () {
        var self = this;
        setTimeout(function () {
          self.autofocus = true;
        }, 1000);
      },
      onchange: function (event) {
        this.txtChange = event.value;
        console.log('onchange', event.value);
      },
      onreturn: function (event) {
        this.txtReturnType = event.returnKeyType;
        console.log('onreturn', event.type);
      },
      oninput: function (event) {
        this.txtInput = event.value;
        console.log('oninput', event.value);
      },
      focus: function () {
        this.$refs['input1'].focus();
      },
      blur: function () {
        this.$refs['input1'].blur();
      },
      setRange: function() {
        console.log(this.$refs["inputselection"]);
        this.$refs["inputselection"].setSelectionRange(2, 6);
      },
      getSelectionRange: function() {
        console.log(this.$refs["inputselection"]);
        var self = this;
        this.$refs["inputselection"].getSelectionRange(function(e) {
          self.txtSelection = e.selectionStart +'-' + e.selectionEnd;
        });
      }
    }
  };
</script>
```

---
### \<list\>
行为与TableView(RecycleView)一致，但不具有复用机制，展示纵向列表。

#### 子组件

- \<cell\>
- \<header\>
- \<refresh\>
- \<loading\>

#### 特性

- loadmoreoffset
- offset-accuracy

#### 事件

- loadmore
- scroll

#### 扩展

- scrollToElement(node, options)
- resetLoadmore()

示例代码：

```
<template>
  <list class="list" @loadmore="fetch" loadmoreoffset="10">
    <cell class="cell" v-for="num in lists">
      <div class="panel">
        <text class="text">{{num}}</text>
      </div>
    </cell>
  </list>
</template>

<script>
  const modal = weex.requireModule('modal')
  const LOADMORE_COUNT = 4

  export default {
    data () {
      return {
        lists: [1, 2, 3, 4, 5]
      }
    },
    methods: {
      fetch (event) {
        modal.toast({ message: 'loadmore', duration: 1 })

        setTimeout(() => {
          const length = this.lists.length
          for (let i = length; i < length + LOADMORE_COUNT; ++i) {
            this.lists.push(i + 1)
          }
        }, 800)
      }
    }
  }
</script>

<style scoped>
  .panel {
    width: 600px;
    height: 250px;
    margin-left: 75px;
    margin-top: 35px;
    margin-bottom: 35px;
    flex-direction: column;
    justify-content: center;
    border-width: 2px;
    border-style: solid;
    border-color: rgb(162, 217, 192);
    background-color: rgba(162, 217, 192, 0.2);
  }
  .text {
    font-size: 50px;
    text-align: center;
    color: #41B883;
  }
</style>
```

---
### \<cell\>

作为\<list\> 、\<recycler\>、 \<waterfall.\>的子组件存在，本身可以包含所有Weex组件作为其子组件。

#### 特性
- keep-scroll-position

> 由于 <cell> 本身是一个容器，其布局由 <list> 进行管理，你不能给 <cell> 设定flex值。 <cell>的宽度等于父组件 <list> 的宽度，并且 <cell> 高度自适应，指定 margin 样式也不起作用。

---
### \<recycle-list\>

与\<list\>行为相似，配合\<cell-slot\>实现复用机制。

```
<recycle-list for="(item, i) in longList" switch="type">
  <cell-slot case="A">
    <text>- A {{i}} -</text>
  </cell-slot>
  <cell-slot case="B">
    <text>- B {{i}} -</text>
  </cell-slot>
</recycle-list>

const longList = [
  { type: 'A' },
  { type: 'B' },
  { type: 'B' },
  { type: 'A' },
  { type: 'B' }
]
```

通过for循环确定数据源，通过switch选择cell样式。

具体\<recycle-list\>还在开发之中，目前建议使用\<list\>，资料后续补齐。

---
### \<loading\>

提供上拉加载功能。

示例代码：

```
<template>
  <scroller class="scroller">
    <div class="cell" v-for="num in lists">
      <div class="panel">
        <text class="text">{{num}}</text>
      </div>
    </div>
    <loading class="loading" @loading="onloading" :display="loadinging ? 'show' : 'hide'">
      <text class="indicator-text">Loading ...</text>
      <loading-indicator class="indicator"></loading-indicator>
    </loading>
  </scroller>
</template>

<script>
  const modal = weex.requireModule('modal')
  const LOADMORE_COUNT = 4
  export default {
    data () {
      return {
        loadinging: false,
        lists: [1, 2, 3, 4, 5]
      }
    },
    methods: {
      onloading (event) {
        modal.toast({ message: 'Loading', duration: 1 })
        this.loadinging = true
        setTimeout(() => {
          this.loadinging = false
          const length = this.lists.length
          for (let i = length; i < length + LOADMORE_COUNT; ++i) {
            this.lists.push(i + 1)
          }
        }, 2000)
      },
    }
  }
</script>

<style scoped>
  .loading {
    width: 750;
    display: -ms-flex;
    display: -webkit-flex;
    display: flex;
    -ms-flex-align: center;
    -webkit-align-items: center;
    -webkit-box-align: center;
    align-items: center;
  }
  .indicator-text {
    color: #888888;
    font-size: 42px;
    text-align: center;
  }
  .indicator {
    margin-top: 16px;
    height: 40px;
    width: 40px;
    color: blue;
  }
  .panel {
    width: 600px;
    height: 250px;
    margin-left: 75px;
    margin-top: 35px;
    margin-bottom: 35px;
    flex-direction: column;
    justify-content: center;
    border-width: 2px;
    border-style: solid;
    border-color: #DDDDDD;
    background-color: #F5F5F5;
  }
  .text {
    font-size: 50px;
    text-align: center;
    color: #41B883;
  }
</style>
```

---
### \<refresh\>

提供下拉刷新功能，用法上类似\<loading\>。

#### 特性

- display
	- show
	如果 <refresh> 中包含 <loading-indicator>，则将其显示并开始默认动画。
	- hide
	收起 refresh view，如果 <refresh> 中包含 <loading-indicator>，则将其视图隐藏。
	
> display 的设置必须成对出现，即设置 display="show",必须有对应的 display="hide"。

#### 事件
- refresh
- pullingdown

示例代码：

```
<template>
  <scroller class="scroller">
    <refresh class="refresh" @refresh="onrefresh" @pullingdown="onpullingdown" :display="refreshing ? 'show' : 'hide'">
      <text class="indicator-text">Refreshing ...</text>
      <loading-indicator class="indicator"></loading-indicator>
    </refresh>
    <div class="cell" v-for="num in lists">
      <div class="panel">
        <text class="text">{{num}}</text>
      </div>
    </div>
  </scroller>
</template>

<script>
  const modal = weex.requireModule('modal')

  export default {
    data () {
      return {
        refreshing: false,
        lists: [1, 2, 3, 4, 5]
      }
    },
    methods: {
      onrefresh (event) {
        modal.toast({ message: 'Refreshing', duration: 1 })
        this.refreshing = true
        setTimeout(() => {
          this.refreshing = false
        }, 2000)
      },
      onpullingdown (event) {
        console.log("dy: " + event.dy)
        console.log("pullingDistance: " + event.pullingDistance)
        console.log("viewHeight: " + event.viewHeight)
        console.log("type: " + type)
      }
    }
  }
</script>

<style scoped>
  .refresh {
    width: 750;
    display: -ms-flex;
    display: -webkit-flex;
    display: flex;
    -ms-flex-align: center;
    -webkit-align-items: center;
    -webkit-box-align: center;
    align-items: center;
  }
  .indicator-text {
    color: #888888;
    font-size: 42px;
    text-align: center;
  }
  .indicator {
    margin-top: 16px;
    height: 40px;
    width: 40px;
    color: blue;
  }
  .panel {
    width: 600px;
    height: 250px;
    margin-left: 75px;
    margin-top: 35px;
    margin-bottom: 35px;
    flex-direction: column;
    justify-content: center;
    border-width: 2px;
    border-style: solid;
    border-color: #DDDDDD;
    background-color: #F5F5F5;
  }
  .text {
    font-size: 50px;
    text-align: center;
    color: #41B883;
  }
</style>
```

---
### \<scroller\>
行为上与ScrollView相似。

#### 特性

- show-scrollbar

- scroll-direction 
可选为 horizontal 或者 vertical，默认值为 vertical 。定义滚动的方向。

	> scroll-direction定义了 scroller 的滚动方向，样式表属性 flex-direction 定义了 scroller 的布局方向，两个方向必须一致。
	> scroll-direction 的默认值是 vertical, flex-direction 的默认值是 row。
	> 当需要一个水平方向的 scroller 时，使用 scroll-direction:horizontal 和 flex-direction: row。
	> 当需要一个竖直方向的 scroller 时，使用 scroll-direction:vertical 和 flex-direction: column。由于这两个值均是默认值，当需要一个竖直方向的 scroller 时，这两个值可以不设置。

- loadmoreoffset
- offset-accuracy

#### 事件及扩展
与\<list\>相同。

示例代码：

```
<template>
  <div class="wrapper">
    <scroller class="scroller">
      <div class="row" v-for="(name, index) in rows" :ref="'item'+index">
        <text class="text" :ref="'text'+index">{{name}}</text>
      </div>
    </scroller>
    <div class="group">
      <text @click="goto10" class="button">Go to 10</text>
      <text @click="goto20" class="button">Go to 20</text>
    </div>
  </div>
</template>

<script>
  const dom = weex.requireModule('dom')

  export default {
    data () {
      return {
        rows: []
      }
    },
    created () {
      for (let i = 0; i < 30; i++) {
        this.rows.push('row ' + i)
      }
    },
    methods: {
      goto10 (count) {
        const el = this.$refs.item10[0]
        dom.scrollToElement(el, {})
      },
      goto20 (count) {
        const el = this.$refs.item20[0]
        dom.scrollToElement(el, { offset: 0 })
      }
    }
  }
</script>

<style scoped>
  .scroller {
    width: 700px;
    height: 700px;
    border-width: 3px;
    border-style: solid;
    border-color: rgb(162, 217, 192);
    margin-left: 25px;
  }
  .row {
    height: 100px;
    flex-direction: column;
    justify-content: center;
    padding-left: 30px;
    border-bottom-width: 2px;
    border-bottom-style: solid;
    border-bottom-color: #DDDDDD;
  }
  .text {
    font-size: 45px;
    color: #666666;
  }
  .group {
    flex-direction: row;
    justify-content: center;
    margin-top: 60px;
  }
  .button {
    width: 200px;
    padding-top: 20px;
    padding-bottom: 20px;
    font-size: 40px;
    margin-left: 30px;
    margin-right: 30px;
    text-align: center;
    color: #41B883;
    border-width: 2px;
    border-style: solid;
    border-color: rgb(162, 217, 192);
    background-color: rgba(162, 217, 192, 0.2);
  }
</style>
```

---
### \<slider\>

轮播图。

> 用于显示轮播图指示器效果，\<indicator\>必须充当 \<slider\> 组件的子组件使用。

#### 特性

- auto-play 
- interval
- infinite//循环播放，可选值为 true/false，默认的是 true。
- offset-x-accuracy//控制onscroll事件触发的频率，默认值为10，表示两次onscroll事件之间Slider Page至少滚动了10px。注意，将该值设置为较小的数值会提高滚动事件采样的精度，但同时也会降低页面的性能。
- show-indicators
- index
- scrollable
- keep-index

#### 事件
- change
	
示例代码：

```
<template>
  <div>
    <slider class="slider" interval="3000" auto-play="true">
      <div class="frame" v-for="img in imageList">
        <image class="image" resize="cover" :src="img.src"></image>
      </div>
    </slider>
  </div>
</template>

<style scoped>
  .image {
    width: 700px;
    height: 700px;
  }
  .slider {
    margin-top: 25px;
    margin-left: 25px;
    width: 700px;
    height: 700px;
    border-width: 2px;
    border-style: solid;
    border-color: #41B883;
  }
  .frame {
    width: 700px;
    height: 700px;
    position: relative;
  }
</style>

<script>
  export default {
    data () {
      return {
        imageList: [
          { src: 'https://gd2.alicdn.com/bao/uploaded/i2/T14H1LFwBcXXXXXXXX_!!0-item_pic.jpg'},
          { src: 'https://gd1.alicdn.com/bao/uploaded/i1/TB1PXJCJFXXXXciXFXXXXXXXXXX_!!0-item_pic.jpg'},
          { src: 'https://gd3.alicdn.com/bao/uploaded/i3/TB1x6hYLXXXXXazXVXXXXXXXXXX_!!0-item_pic.jpg'}
        ]
      }
    }
  }
</script>
```

---
### \<text\>

\<text\> 是 Weex 内置的组件，用来将文本按照指定的样式渲染出来。\<text\> 只能包含文本值，你可以使用 {{}} 标记插入变量值作为文本内容。

> 注意： <text> 里直接写文本头尾空白会被过滤，如果需要保留头尾空白，暂时只能通过数据绑定写头尾空格。

> 注意： <text>不支持子组件。

#### 特性

- lines
- 文本样式
	- color
	- font-size
	- font-style
	- font-weight
	- text-align
	- text-decoration
	- text-overflow 
	- line-height

---
### \<textarea\>

\<textarea\> 是 Weex 内置的一个组件，用于用户交互，接受用户输入数据。 可以认为是允许多行的 \<input\>。

> textarea 组件不支持子组件。

#### 特性
- text所有特性
- rows

#### 事件

\<input\>所有事件。

```
<template>
  <div class="wrapper">
    <textarea class="textarea" @input="oninput" @change="onchange" @focus="onfocus" @blur="onblur"></textarea>
  </div>
</template>

<script>
  const modal = weex.requireModule('modal')

  export default {
    methods: {
      oninput (event) {
        console.log('oninput:', event.value)
        modal.toast({
          message: `oninput: ${event.value}`,
          duration: 0.8
        })
      },
      onchange (event) {
        console.log('onchange:', event.value)
        modal.toast({
          message: `onchange: ${event.value}`,
          duration: 0.8
        })
      },
      onfocus (event) {
        console.log('onfocus:', event.value)
        modal.toast({
          message: `onfocus: ${event.value}`,
          duration: 0.8
        })
      },
      onblur (event) {
        console.log('onblur:', event.value)
        modal.toast({
          message: `input blur: ${event.value}`,
          duration: 0.8
        })
      }
    }
  }
</script>

<style>
  .textarea {
    font-size: 50px;
    width: 650px;
    margin-top: 50px;
    margin-left: 50px;
    padding-top: 20px;
    padding-bottom: 20px;
    padding-left: 20px;
    padding-right: 20px;
    color: #666666;
    border-width: 2px;
    border-style: solid;
    border-color: #41B883;
  }
</style>
```

---
### \<video\>

\<video\> 组件可以让我们在 Weex 页面中嵌入视频内容。

> \<text\> 是唯一合法的子组件。

#### 特性
- src {string}：内嵌的视频指向的URL
- play-status {string}：可选值为 play | pause，用来控制视频的播放状态，play 或者 pause，默认值是 pause。
- auto-play {boolean}：可选值为 true | false，当页面加载初始化完成后，用来控制视频是否立即播放，默认值是 false。

#### 事件

- start
- pause
- finish
- fail

示例代码：

```
<template>
  <div>
    <video class="video" :src="src" autoplay controls
      @start="onstart" @pause="onpause" @finish="onfinish" @fail="onfail"></video>
    <text class="info">state: {{state}}</text>
  </div>
</template>

<style scoped>
  .video {
    width: 630px;
    height: 350px;
    margin-top: 60px;
    margin-left: 60px;
  }
  .info {
    margin-top: 40px;
    font-size: 40px;
    text-align: center;
  }
</style>

<script>
  export default {
    data () {
      return {
        state: '----',
        src:'http://flv2.bn.netease.com/videolib3/1611/01/XGqSL5981/SD/XGqSL5981-mobile.mp4'
      }
    },
    methods:{
      onstart (event) {
        this.state = 'onstart'
      },
      onpause (event) {
        this.state = 'onpause'
      },
      onfinish (event) {
        this.state = 'onfinish'
      },
      onfail (event) {
        this.state = 'onfinish'
      }
    }
  }
</script>
```

---
### \<waterfall\>

提供瀑布流布局。与\<list\>子组件相同。

#### 特性

- column-width : 描述瀑布流每一列的列宽
	- auto: 意味着列宽是被其他属性所决定的(比如 column-count)
	- <length>: 最佳列宽，实际的列宽可能会更宽(需要填充剩余的空间)， 或者更窄(如果剩余空间比列宽还要小)。 该值必须大于0
- column-count: 描述瀑布流的列数
	- auto: 意味着列数是被其他属性所决定的(比如 column-width)
	- <integer>: 最佳列数，column-width 和 column-count 都指定非0值， 则 column-count 代表最大列数。
- column-gap: 列与列的间隙. 如果指定了 normal ，则对应 32.
- left-gap: 左边cell和列表的间隙. 如果未指定 ，则对应 0v0.19+.
- column-gap: 右边cell和列表的间隙. 如果未指定，则对应 0v0.19+.

示例代码：

```
<template>
  <waterfall class="page" ref="waterfall"
  v-bind:style="{padding:padding}"
  :column-width="columnWidth" :column-count="columnCount" :column-gap="columnGap"
  :show-scrollbar="showScrollbar" :scrollable="scrollable"
  @scroll="recylerScroll" @loadmore="loadmore" loadmoreoffset=3000
  >
  <!--<refresh class="refresh" @refresh="onrefresh" @pullingdown="onpullingdown" :display="refreshing ? 'show' : 'hide'">
      <loading-indicator class="indicator"></loading-indicator>
      <text class="refreshText">{{refreshText}}</text>
  </refresh>-->
    <header class="header" ref="header" v-if="showHeader">
      <div class="banner">
       <image class="absolute" src="https://gw.alicdn.com/tps/TB1ESN1PFXXXXX1apXXXXXXXXXX-1000-600.jpg" resize="cover"></image>
       <div class="bannerInfo">
          <image class="avatar" src="https://gw.alicdn.com/tps/TB1EP9bPFXXXXbpXVXXXXXXXXXX-150-110.jpg" resize="cover"></image>
          <text class="name">Adam Cat</text>
          <div class="titleWrap">
            <text class="title">Genius</text>
          </div>
        </div>
        <div class="bannerPhotoWrap">
          <image class="bannerPhoto" v-for="photo in banner.photos" :src="photo.src"></image>
        </div>
      </div>
    </header>
    <header class="stickyHeader" >
      <div v-if="stickyHeaderType === 'none'" class="stickyWrapper">
        <text class="stickyText">Sticky Header</text>
      </div>
      <div v-if="stickyHeaderType === 'appear'" class="stickyWrapper">
        <div class="stickyTextImageWrapper">
          <text class="stickyText">Last Appear:</text>
          <image class="stickyImage" :src="appearImage"></image>
        </div>
        <div class="stickyTextImageWrapper">
          <text class="stickyText">Last Disappear:</text>
          <image class="stickyImage" :src="disappearImage"></image>
        </div>
      </div>
      <div v-if="stickyHeaderType === 'scroll'" class="stickyWrapper">
        <text class="stickyText">Content Offset:{{contentOffset}}</text>
      </div>
    </header>
    <cell v-for="(item, index) in items" :key="item.src" class="cell" ref="index">
      <div class="item" @click="onItemclick(item.behaviour, index)" @appear="itemAppear(item.src)" @disappear="itemDisappear(item.src)">
        <text v-if="item.name" class="itemName">{{item.name}}</text>
        <image class="itemPhoto" :src="item.src"></image>
        <text v-if="item.desc" class="itemDesc">{{item.desc}}</text>
        <text v-if="item.behaviourName" class="itemClickBehaviour"> {{item.behaviourName}}</text>
      </div>
    </cell>
    <header class="footer" ref="footer">
      <text class="stickyText">Footer</text>
    </header>
    <div ref="fixed" class="fixedItem" @click="scrollToNext">
      <text class="fixedText">bot</text>
    </div>
  </waterfall>
</template>

<style>
  .page {
    background-color: #EFEFEF;
  }
  .refresh {
    height: 128;
    width: 750;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }
  .refreshText {
    color: #888888;
    font-weight: bold;
  }
  .indicator {
    color: #888888;
    height: 40;
    width: 40;
    margin-right: 30;
  }
  .absolute {
  position: absolute;
  top: 0px;
  width: 750;
  height: 377;
}
  .banner {
    height: 377;
    flex-direction: row;
  }
  .bannerInfo {
    width:270;
    align-items: center;
    justify-content: center;
  }
  .avatar {
    width: 148;
    height: 108;
    border-radius: 54;
    border-width: 4;
    border-color: #FFFFFF;
    margin-bottom: 14;
  }
  .name {
    font-weight: bold;
    font-size:32;
    color:#ffffff;
    line-height:32;
    text-align:center;
    margin-bottom: 16;
  }
  .titleWrap {
    width: 100;
    height: 24;
    margin-bottom: 10;
    background-color: rgba(255,255,255,0.80);
    border-radius: 12;
    justify-content: center;
    align-items: center;
  }
  .title {
    font-size: 20;
    color: #000000;
  }
  .bannerPhotoWrap {
    width: 449;
    height: 305;
    background-color: #FFFFFF;
    border-radius: 12;
    margin-top: 35;
    padding: 12;
    flex-direction: row;
    justify-content: space-between;
    flex-wrap:wrap;
  }
  .bannerPhoto {
    width: 137;
    height: 137;
    margin-bottom: 6;
  }
  .stickyHeader {
    position: sticky;
    height: 94;
    flex-direction: row;
    padding-bottom:6;
  }
  .stickyWrapper {
    flex-direction: row;
    background-color:#00cc99;
    justify-content: center;
    align-items: center;
    flex:1;
  }
  .stickyTextImageWrapper {
    flex:1;
    justify-content: center;
    align-items: center;
    flex-direction: row;
  }
  .stickyText {
    color: #FFFFFF;
    font-weight: bold;
    font-size:32;
    margin-right: 12;
  }
  .stickyImage {
    width: 64;
    height: 64;
    border-radius: 32;
  }

  .cell {
    padding-top: 6;
    padding-bottom: 6;
  }
  .item {
    background-color: #FFFFFF;
    align-items: center;
  }
  .itemName {
    font-size:28;
    color:#333333;
    line-height:42;
    text-align:left;
    margin-top: 24;
  }
  .itemPhoto {
    margin-top: 18;
    width: 220;
    height: 220;
    margin-bottom: 18;
  }
  .itemDesc {
    font-size:24;
    margin:12;
    color:#999999;
    line-height:36;
    text-align:left;
  }
  .itemClickBehaviour {
    font-size:36;
    color:#00cc99;
    line-height:36;
    text-align:center;
    margin-top: 6;
    margin-left: 24;
    margin-right: 24;
    margin-bottom: 30;
  }
  .footer {
    height: 94;
    justify-content: center;
    align-items: center;
    background-color: #00cc99;
  }

  .fixedItem {
    position: fixed;
    width:78;
    height:78;
    background-color:#00cc99;
    right: 32;
    bottom: 32;
    border-radius: 39;
    align-items: center;
    justify-content: center;
  }
  .fixedText {
    font-size: 32;
    color: white;
    line-height: 32;
  }

</style>

<script>
  export default {
    data: function() {
      const items = [
        {
          src:'https://gw.alicdn.com/tps/TB1Jl1CPFXXXXcJXXXXXXXXXXXX-370-370.jpg',
          name: 'Thomas Carlyle',
          desc:'Genius only means hard-working all one\'s life',
          behaviourName: 'Change width',
          behaviour: 'changeColumnWidth',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1Hv1JPFXXXXa3XXXXXXXXXXXX-370-370.jpg',
          desc:'The man who has made up his mind to win will never say "impossible "',
          behaviourName: 'Change gap',
          behaviour: 'changeColumnGap'
        },
        {
          src:'https://gw.alicdn.com/tps/TB1eNKuPFXXXXc_XpXXXXXXXXXX-370-370.jpg',
          desc:'There is no such thing as a great talent without great will - power',
          behaviourName: 'Change count',
          behaviour: 'changeColumnCount'
        },
        {
          src:'https://gw.alicdn.com/tps/TB1DCh8PFXXXXX7aXXXXXXXXXXX-370-370.jpg',
          name:'Addison',
          desc:'Cease to struggle and you cease to live',
          behaviourName: 'Show scrollbar',
          behaviour: 'showScrollbar',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1ACygPFXXXXXwXVXXXXXXXXXX-370-370.jpg',
          desc:'A strong man will struggle with the storms of fate',
          behaviourName: 'Listen appear',
          behaviour: 'listenAppear',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1IGShPFXXXXaqXVXXXXXXXXXX-370-370.jpg',
          name:'Ruskin',
          desc:'Living without an aim is like sailing without a compass',
          behaviourName: 'Set scrollable',
          behaviour: 'setScrollable',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1xU93PFXXXXXHaXXXXXXXXXXX-240-240.jpg',
          behaviourName: 'waterfall padding',
          behaviour: 'setPadding',
        },
        {
          src:'https://gw.alicdn.com/tps/TB19hu0PFXXXXaXaXXXXXXXXXXX-240-240.jpg',
          name:'Balzac',
          desc:'There is no such thing as a great talent without great will - power',
          behaviourName: 'listen scroll',
          behaviour: 'listenScroll',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1ux2vPFXXXXbkXXXXXXXXXXXX-240-240.jpg',
          behaviourName: 'Remove cell',
          behaviour: 'removeCell',
        },
        {
          src:'https://gw.alicdn.com/tps/TB1tCCWPFXXXXa7aXXXXXXXXXXX-240-240.jpg',
          behaviourName: 'Move cell',
          behaviour: 'moveCell',
        }
      ]

      let repeatItems = [];
      for (let i = 0; i < 3; i++) {
        repeatItems.push(...items)
      }

      return {
        padding: 0,
        refreshing: false,
        refreshText: '↓   pull to refresh...',
        columnCount: 2,
        columnGap: 12,
        columnWidth: 'auto',
        contentOffset: '0',
        showHeader: true,
        showScrollbar: false,
        scrollable: true,
        showStickyHeader: false,
        appearImage: null,
        disappearImage: null,
        stickyHeaderType: 'none',
        // fixedRect:'',
        banner: {
          photos: [
            {src:'https://gw.alicdn.com/tps/TB1JyaCPFXXXXc9XXXXXXXXXXXX-140-140.jpg'},
            {src:'https://gw.alicdn.com/tps/TB1MwSFPFXXXXbdXXXXXXXXXXXX-140-140.jpg'},
            {src:'https://gw.alicdn.com/tps/TB1U8avPFXXXXaDXpXXXXXXXXXX-140-140.jpg'},
            {src:'https://gw.alicdn.com/tps/TB17Xh8PFXXXXbkaXXXXXXXXXXX-140-140.jpg'},
            {src:'https://gw.alicdn.com/tps/TB1cTmLPFXXXXXRXXXXXXXXXXXX-140-140.jpg'},
            {src:'https://gw.alicdn.com/tps/TB1oCefPFXXXXbVXVXXXXXXXXXX-140-140.jpg'}
          ]
        },
        items: repeatItems
      }
    },

    created() {
      // let self = this
      // setTimeout(()=>{
      //   weex.requireModule('dom').getComponentRect(this.$refs.fixed, result=>{
      //     const x = result.size.left
      //     const y = result.size.top
      //     const width = result.size.width
      //     const height = result.size.height
      //     self.fixedRect = `${x}|${y}|${width}|${height}`
      //   })
      // }, 3000)
    },

    methods: {
      recylerScroll: function(e) {
        this.contentOffset = e.contentOffset.y
      },
      loadmore: function(e) {
        console.log('receive loadmore event')
        // this.$refs.waterfall.resetLoadmore()
      },
      showOrRemoveHeader: function() {
        this.showHeader = !this.showHeader
      },
      onItemclick: function (behaviour, index) {
        console.log(`click...${behaviour} at index ${index}`)
        switch (behaviour) {
          case 'changeColumnCount':
            this.changeColumnCount()
            break
          case 'changeColumnGap':
            this.changeColumnGap()
            break
          case 'changeColumnWidth':
            this.changeColumnWidth()
            break
          case 'showScrollbar':
            this.showOrHideScrollbar()
            break
          case 'listenAppear':
            this.listenAppearAndDisappear()
            break
          case 'setScrollable':
            this.setScrollable()
            break
          case 'setPadding':
            this.setRecyclerPadding()
            break
          case 'listenScroll':
            this.listenScrollEvent()
            break
          case 'removeCell':
            this.removeCell(index)
            break
          case 'moveCell':
            this.moveCell(index)
            break
        }
      },

      itemAppear: function(src) {
        this.appearImage = src;
      },

      itemDisappear: function(src) {
        this.disappearImage = src;
      },

      changeColumnCount: function() {
        if (this.columnCount === 2) {
          this.columnCount = 3
        } else {
          this.columnCount = 2
        }
      },

      changeColumnGap: function() {
        if (this.columnGap === 12) {
          this.columnGap = 'normal'
        } else {
          this.columnGap = 12
        }
      },

      changeColumnWidth: function() {
        if (this.columnWidth === 'auto') {
          this.columnWidth = 600
        } else {
          this.columnWidth = 'auto'
        }
      },

      showOrHideScrollbar: function() {
        this.showScrollbar = !this.showScrollbar
      },

      setScrollable: function() {
        this.scrollable = !this.scrollable
      },

      listenAppearAndDisappear: function() {
        this.stickyHeaderType = (this.stickyHeaderType === 'appear' ? 'none' : 'appear')
      },

      listenScrollEvent: function() {
        this.stickyHeaderType = (this.stickyHeaderType === 'scroll' ? 'none' : 'scroll')
      },

      scrollToNext: function() {
        weex.requireModule('dom').scrollToElement(this.$refs.footer)
      },

      setRecyclerPadding: function() {
        this.padding = (this.padding == 0 ? 12 : 0);
      },

      removeCell: function(index) {
        this.items.splice(index, 1)
      },

      moveCell: function(index) {
        if (index == 0) {
          this.items.splice(this.items.length - 1, 0, this.items.splice(index, 1)[0]);
        } else {
          this.items.splice(0, 0, this.items.splice(index, 1)[0]);
        }
      },

      onrefresh (event) {
        this.refreshing = true
        this.refreshText = "loading..."
        setTimeout(() => {
          this.refreshing = false
          this.refreshText = '↓   pull to refresh...'
        }, 2000)
      },

      onpullingdown (event) {
        // console.log(`${event.pullingDistance}`)
        if (event.pullingDistance < -64) {
          this.refreshText = '↑   release to refresh...'
        } else {
          this.refreshText = '↓   pull to refresh...'
        }
      }
    }
  }
</script>
```

---
### \<web\>


\<web\> 用于在 weex 页面中显示由 src 属性指定的页面内容。您还可以使用 webview 模块来控制 WebView 的行为，例如回退、前进和重新加载。

> 注意： <web> 不支持任何嵌套的子组件，并且必须指定 width 和 height 的样式属性，否则将不起作用。

#### 特性

- src

#### 事件

- pagestart
- pagefinish
- error
- receivedtitle//仅限安卓平台

#### 注意事项

> 必须指定 \<web\> 的 width 和 height 样式。
> \<web\> 不能包含任何嵌套的子组件。
> 您可以使用 webview module 来控制 <web> 组件。

示例代码：

```
<template>
  <div class="wrapper">
    <web ref="webview" style="width: 730px; height: 500px" src="https://vuejs.org"
      @pagestart="onPageStart" @pagefinish="onPageFinish" @error="onError" @receivedtitle="onReceivedTitle"></web>
    <div class="row" style="padding-top: 10px">
      <text class="button" :class="[canGoBack ? 'button-enabled' : 'button-disabled']" @click="goBack">←</text>
      <text class="button" :class="[canGoForward ? 'button-enabled' : 'button-disabled']" @click="goForward">→</text>
      <text class="button" @click="reload">reload</text>
    </div>
    <text test-id='pagestart'>pagestart: {{pagestart}}</text>
    <text test-id='pagefinish'>pagefinish: {{pagefinish}}</text>
    <text test-id='title'>title: {{title}}</text>
    <text test-id='error'>error: {{error}}</text>
  </div>
</template>

<style scoped>
  .wrapper {
		flex-direction: column;
		padding: 10px;
	}
	.row {
	  flex-direction: row;
	  justify-content: space-between
	}
	.button {
		color: #fff;
		background-color: #337ab7;
		border-color: #2e6da4;
		border-radius: 12px;
		padding-top: 20px;
		padding-left: 36px;
		padding-bottom: 20px;
		padding-right: 36px;
		font-size: 36px;
		text-align: center;
		font-weight: 500;
		margin-bottom: 10px;
  }
  .button-enabled {
    opacity: 1;
  }
  .button-disabled {
    opacity: 0.65;
  }
</style>

<script>
  module.exports = {
    data: {
      pagestart: '',
      pagefinish: '',
      title: '',
      error: '',
      canGoBack: false,
      canGoForward: false,
    },
    methods: {
      goBack: function() {
        var webview = weex.requireModule('webview');
        webview.goBack(this.$refs.webview);
      },
      goForward: function() {
        var webview = weex.requireModule('webview');
        webview.goForward(this.$refs.webview);
      },
      reload: function() {
        var webview = weex.requireModule('webview');
        webview.reload(this.$refs.webview);
      },
      onPageStart: function(e) {
        this.pagestart = e.url;
      },
      onPageFinish: function(e) {
        this.pagefinish = e.url;
        this.canGoBack = e.canGoBack;
        this.canGoForward = e.canGoForward;
        if (e.title) {
          this.title = e.title;
        }
      },
      onError: function(e) {
        this.error = url;
      },
      onReceivedTitle: function(e) {
        this.title = e.title;
      }
    }
  }
</script>
```
