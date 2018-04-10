# Vue

---

### Vue实例结构
```
var vm = new Vue({
  //样式表名
  el: '#example',
  //属性
  data: {
    message: 'Hello'
  },
  //计算属性
  computed: {
    fullName: {
    	// getter
    	get: function () {
      		return this.firstName + ' ' + this.lastName
    	},
    	// setter
    	set: function (newValue) {
      		var names = newValue.split(' ')
      		this.firstName = names[0]
      		this.lastName = names[names.length - 1]
    	}
  	}
  },
  //属性观察
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  //实例函数
  methods: {
  	reversedMessage: function () {
    	return this.message.split('').reverse().join('')
  	}
  }
})
```

### Vue监听实例

```
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>

<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为判定用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```

### Vue实例生命周期
> beforeCreate->created->beforeMount->mounted->beforeUpdate->updated->beforeDestroy->destryed

### v-bind

绑定标签字段与Vue对应关系

```
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```

### v-on
绑定事件与Vue对应关系

```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```

### Vue组件

```
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

### Class绑定

普通

```
<div v-bind:class="{ active: isActive }"></div>
```

数组

```
HTML：
<div v-bind:class="[activeClass, errorClass]"></div>

JS：
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

条件

```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### Style绑定

```
HTML:
<div v-bind:style="styleObject"></div>

JS:
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

### v-if

```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

关于是否显示，v-show同样可以达到相同效果，不同的是
> v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。

> 注意，v-show 不支持 \<template\> 元素，也不支持 v-else。

### v-if 与v-show

> v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

> v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

> 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

> 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

### v-for

```
HTML:
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>

JS:
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

对于对象同样可以使用v-for语法

```
HTML:
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>

JS:
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```

当v-for配合v-if同时使用时，相当于在for循环中添加if判断。

> 2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。
eg.:

```
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```

### TODO LIST的完成例子

```
HTML:
<div id="todo-list-example">
  <input
    v-model="newTodoText"
    v-on:keyup.enter="addNewTodo"
    placeholder="Add a todo"
  >
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>

JS:
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">X</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})

```

### 关于数组

Vue为数组提供了很多变异方法，一边观察数组的变化从而更新试图：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

同时，Vue也提供了一些非变异方法，他们不会改变原数组的值而是返回一个新的数组：

- filter()
- concat()
- slice()

在使用上，我们可以用一个新数组替换旧数组，Vue内部做了优化，提高渲染效率。

> 值得注意的是，由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

>当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
>当你修改数组的长度时，例如：vm.items.length = newLength

应对第一种问题我们可以使用以下三种方式进行值得替换：

```
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
// Vue.set的全局缩写
vm.$set(vm.items, indexOfItem, newValue)
```

第二种问题解决方案：

```
vm.items.splice(newLength)
```

不仅是数组，对象同样会有Vue检测不到的变化：

> Vue 不能检测对象属性的添加或删除

为了解决这个问题我呢们可以调用以下函数：

添加

```
Vue.set(vm.userProfile, 'age', 27)
vm.$set(vm.userProfile, 'age', 27)
```

添加多个值时请使用新对象替换就对象的方式：

```
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

### 关于Vue中的事件绑定

v-on可以用于给事件绑定回调，回调可以是一个表达式：

```
HTML:
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>

JS:
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

同样可以是一个方法：

```
HTML:
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>

JS:
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
```

另外，greet方法是可以以JS形势直接调用的

```
example2.greet()
```

由于此种特性，事件绑定还有一种变形：

```
HTML:
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>

JS:
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

此外，Vue为v-on提供了一系列事件修饰符，用来定制事件的传递

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

### 按键修饰符

- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

```
<!-- 同上 -->
<input v-on:keyup.enter="submit">
```

### v-model

输入框

```
HTML:
<div class="app-8">
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
</div>

JS:
var app8 = new Vue({
  el: '.app-8',
  data: {
    message: 'Hello Vue!'
  }
})
```

复选框(单个)：

```
HTML:
<div class = "app-9">
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">{{ checked }}</label>
</div>

JS:
var app9 = new Vue({
  el: '.app-9',
  data: {
    checked: true
  }
})
```

复选框(多个):

```
HTML:
<div class='app-10'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>

JS:
var app10 = new Vue({
  el: '.app-10',
  data: {
    checkedNames: []
  }
})
```

单选框：

```
HTML:
<div class="app-11">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>

JS:
var app11 = new Vue({
	el: '.app-11',
	data: {
		picked:""
	}
})
```

选择框：

```
HTML:
<div id="app-12">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>

JS:
new Vue({
  el: '#app-12',
  data: {
    selected: ''
  }
})
```

> 如果 v-model 表达式的初始值未能匹配任何选项，\<select\> 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。

上述选择也可以用v-for来动态创建

```
HTML:
<div class = "app-13">
  <select v-model="selected">
    <option v-for="option in options" v-bind:value="option.value">
      {{ option.text }}
    </option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>

JS:
new Vue({
  el: '.app-13',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```

动态绑定的value值：

```
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

### 修饰符

#### .lazy

在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：

```
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

#### .number

如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：

```
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。

#### .trim

如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：


```
<input v-model.trim="msg">
```

### 组件

#### Vue注册全局组件

```

Vue.component('my-component', {
  // 选项
  template: '<div>A custom component!</div>'
})
```

 组件在注册之后，便可以作为自定义元素 <my-component></my-component> 在一个实例的模板中使用。注意确保在初始化根实例之前注册组件：
 
 ```
 HTML:
 <div id="example">//(此处需要有根实例引用)
  <my-component></my-component>
</div>

JS:
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})

// 创建根实例(此处必须常见根实例，否则无法使用自定义组件)
new Vue({
  el: '#example'
})
 ```
 
 
#### 局部注册

你不必把每个组件都注册到全局。你可以通过某个 Vue 实例/组件的实例选项 components 注册仅在其作用域中可用的组件：

```
var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 将只在父组件模板中可用
    'my-component': Child
  }
})
```

#### data 必须是函数

构造 Vue 实例时传入的各种选项大多数都可以在组件里使用。只有一个例外：data 必须是函数。

```
HTML:
<div id="example-2">
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
</div>

JS:

Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  // 技术上 data 的确是一个函数了，因此 Vue 不会警告，
  // 但是我们却给每个组件实例返回了同一个对象的引用
  data: function () {
    return { counter: 0 }
  }
})

new Vue({
  el: '#example-2'
})
```

#### 使用Prop传递数据

组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。父组件的数据需要通过 prop 才能下发到子组件中。

子组件要显式地用 props 选项声明它预期的数据：

```
HTML:
<!-- HTML 特性是不区分大小写的。所以，当使用的不是字符串模板时，camelCase (驼峰式命名) 的 prop 需要转换为相对应的 kebab-case (短横线分隔式命名) -->
<child my-message="hello!"></child>

JS:
Vue.component('child', {
  // 在 JavaScript 中使用 camelCase
  props: ['myMessage'],
  template: '<span>{{ myMessage }}</span>'
})
```

##### 关于data与Prop
Prop更像是对外属性，外界可以使用，而data中的字段只能作为私有属性供组件自身使用。

#### 动态Prop绑定

```
HTML:
<div id="prop-example-2">
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>

JS:
new Vue({
  el: '#prop-example-2',
  data: {
    parentMsg: 'Message from parent'
  }
})
```

#### 单向数据流

> Prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是反过来不会。这是为了防止子组件无意间修改了父组件的状态，来避免应用的数据流变得难以理解。

> 另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop。如果你这么做了，Vue 会在控制台给出警告。

在两种情况下，我们很容易忍不住想去修改 prop 中数据：

- Prop 作为初始值传入后，子组件想把它当作局部数据来用；

- Prop 作为原始数据传入，由子组件处理成其它数据输出。

对这两种情况，正确的应对方式是：
1.定义一个局部变量，并用 prop 的值初始化它：

```
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}
```

2.定义一个计算属性，处理 prop 的值并返回：

```
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

> 注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。

#### Prop验证

我们可以为组件的 prop 指定验证规则。如果传入的数据不符合要求，Vue 会发出警告。这对于开发给他人使用的组件非常有用。

要指定验证规则，需要用对象的形式来定义 prop，而不能用字符串数组：

```
Vue.component('example', {
  props: {
    // 基础类型检测 (`null` 指允许任何类型)
    propA: Number,
    // 可能是多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数值且有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组/对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

type 可以是下面原生构造器：

- String
- Number
- Boolean
- Function
- Object
- Array
- Symbol

type 也可以是一个自定义构造器函数，使用 instanceof 检测。

当 prop 验证失败，Vue 会抛出警告 (如果使用的是开发版本)。注意 prop 会在组件实例创建之前进行校验，所以在 default 或 validator 函数里，诸如 data、computed 或 methods 等实例属性还无法使用。

### 非Prop特性

尽管为组件定义明确的 prop 是推荐的传参方式，组件的作者却并不总能预见到组件被使用的场景。所以，组件可以接收任意传入的特性，这些特性都会被添加到组件的根元素上。

```
<bs-date-input data-3d-date-picker="true"></bs-date-input>
```

添加属性 data-3d-date-picker="true" 之后，它会被自动添加到 bs-date-input 的根元素上。

### 自定义事件

父控件通过Prop向子控件传递数据，那么子控件又如何与父控件通信呢？
我们可以通过父控件监听子控件事件来实现这一需求，那么就要借助事件接口：

- 使用 $on(eventName) 监听事件
- 使用 $emit(eventName, optionalPayload) 触发事件

eg.:

```
HTML:

<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>

JS:
Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})

new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```

### 非父子组件间通信

事实上$emit和$on只是向我们提供了事件监听的接口，并没有将范围限制在父子控件间，在简单的场景下，可以使用一个空的 Vue 实例作为事件总线：

```
var bus = new Vue()
// 触发组件 A 中的事件
bus.$emit('id-selected', 1)
// 在组件 B 创建的钩子中监听事件
bus.$on('id-selected', function (id) {
  // ...
})
```

