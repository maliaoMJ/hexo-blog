---
title: 阅读Vue Composition API 了解Vue3.x的设计思想（上篇）
catalog: true
tags:
  - JavaScript
  - TypeScript
  - Vue
categories:
  - Vue
date: 2019-10-17 20:39:42
subtitle:
header-img: "https://img.zcool.cn/community/0144ed58660585a8012060c810d41f.jpg@2o.jpg"
---

# API Reference 

> 要看源码之前，首先要学会TypeScript（前十篇文档有`Typescript`学习指南）,因为vue3.x 版本中+90%的代码是使用的`Typescript`，[Vue3.x 源码地址](https://github.com/vuejs/vue-next)。

## Setup
  该`setup`功能是新的组件选项。它充当在组件内部使用Composition API的入口点。
  #### 调用时间
  `setup`创建组件实例时，在初始道具解析后立即调用。在生命周期方面，在`beforeCreate`挂接之后和挂接之前调用它`created`。
  #### 和模板配合使用
  如果`setup`返回一个对象，则该对象上的属性将合并到组件模板的渲染上下文中：

    ```javascript
    <template>
      <div>{{ count }} {{ object.foo }}</div>
    </template>

    <script>
    import { ref, reactive } from 'vue'

    export default {
      setup() {
        const count = ref(0)
        const object = reactive({ foo: 'bar' })

        // expose to template
        return {
          count,
          object
        }
      }
    }
    </script>
    ```
  请注意，`setup`在模板中访问时，从`ref`返回的引用会自动解包，因此不需要`.valuein`模板。
  #### 渲染功能/ JSX的用法
  `setup` 还可以返回一个`render`函数，该函数可以直接使用在同一作用域中声明的反应状态：

    ```javascript
   import { h, ref, reactive } from 'vue'

    export default {
      setup() {
        const count = ref(0)
        const object = reactive({ foo: 'bar' })

        return () => h('div', [
          count.value,
          object.foo
        ])
      }
    }
    ```
  #### 参数
  该函数将接收到的`props`作为其第一个参数：

    ```javascript
      export default {
      props: {
        name: String
      },
      setup(props) {
        console.log(props.name)
      }
    }
    ```
  请注意，此`props`对象是反应性的-即，当传入新的`props`时会更新该对象，并且可以使用`watch`函数观察并做出反应：

   ```javascript
      export default {
      props: {
        name: String
      },
      setup(props) {
        watch(() => {
          console.log(`name is: ` + props.name)
        })
      }
    }
    ```

  `props`在开发过程中，该对象对于用户区代码是不可变的（如果用户代码尝试对其进行更改，则会发出警告）。

  第二个参数提供了一个上下文对象`context`，该对象公开了先前`this`在2.x API中公开的属性的选择性列表：

  ```javascript
    const MyComponent = {
    setup(props, context) {
      context.attrs
      context.slots
      context.parent
      context.root
      context.emit
    }
  }
  ```

`attrs`并且`slots`是内部组件实例上对应值的代理。这样可以确保即使在更新后它们也始终显示最新值，以便我们可以对它们进行结构分解而不必担心访问陈旧的引用：

  ```javascript
    const MyComponent = {
    setup(props, { attrs }) {
      // a function that may get called at a later stage
      function onClick() {
        console.log(attrs.foo) // guaranteed to be the latest reference
      }
    }
  }
  ```

有很多理由将其放置`props`为单独的第一个参数而不是将其包含在上下文中：

- `props`与其他属性相比，使用一个组件更为常见，并且通常一个组件仅使用`props`。
- 具有`props`作为单独的参数可以更容易地单独键入它（见仅打字稿-道具打字下文）而对上下文搞乱了其它类型的属性。通过`TSX`支持`setup`，它还可以跨，`render`和普通功能组件保持一致的签名。

#### This
`this`里面没有`setup()`。由于`setup()`是在解析2.x选项之前调用的，因此`this`内部`setup()`（如果可用）的行为将与`this`其他2.x选项完全不同。`setup()`与其他2.x选项一起使用时，使其可用可能会引起混乱。避免`this`进入的另一个原因`setup()`是对于初学者来说非常常见的陷阱：

 ```javascript
   setup() {
    function onClick() {
      this // not the `this` you'd expect!
    }
  }
  ```
#### Typing(类型)

```typescript
 interface Data {
    [key: string]: unknown
  }

  interface SetupContext {
    attrs: Data
    slots: Slots
    parent: ComponentInstance | null
    root: ComponentInstance
    emit: ((event: string, ...args: unknown[]) => void)
  }

  function setup(
    props: Data,
    context: SetupContext
  ): Data
  ```
> 要获得传递给的参数的类型推断，需要`setup()`使用`createComponent`。


## Reactive
取得一个对象并返回原始对象的反应式代理。这相当于2.x的`Vue.observable()`。

```javascript
const obj = reactive({ count: 0 })
```
反应式转换是“深度”的：它将影响所有嵌套属性。在基于ES2015代理的实现中，返回的代理不等于原始对象。建议仅与反应式代理一起使用，并避免依赖原始对象。
#### typeing 类型

```typescript
   function reactive<T extends object>(raw: T): T
```

## Ref
接受一个内部值并返回一个反应性且可变的`ref`对象。`ref`对象具有`.value`指向内部值的单个属性。

```javascript
  const count = ref(0)
  console.log(count.value) // 0

  count.value++
  console.log(count.value) // 1
```
如果将一个对象分配为`ref`的值，则该`reactive`方法会使该对象具有高度的反应性。

#### 模板中访问
当`ref`作为渲染上下文（从中返回的对象`setup()`）的属性返回并在模板中进行访问时，它会自动展开为内部值。无需`.value`在模板中追加：

```javascript
  <template>
    <div>{{ count }}</div>
  </template>

  <script>
  export default {
    setup() {
      return {
        count: ref(0)
      }
    }
  }
  </script>
```
#### 反应性对象中的访问
当ref作为反应对象的属性进行访问或突变时，它会自动展开为内部值，因此其行为类似于普通属性：

```javascript
const count = ref(0)
const state = reactive({
  count
})

console.log(state.count) // 0

state.count = 1
console.log(count.value) // 1
```
请注意，如果将新的引用分配给链接到现有引用的属性，它将替换旧的引用：

```javascript
const otherCount = ref(2)

state.count = otherCount
console.log(state.count) // 2
console.log(count.value) // 1
```
#### typing 类型

```typescript
interface Ref<T> {
  value: T
}

function ref<T>(value: T): Ref<T>
```
有时我们可能需要为ref的内部值指定复杂类型。我们可以通过在调用ref以覆盖默认推断时传递泛型参数来简洁地实现此目的：

```typescript
const foo = ref<string | number>('foo') // foo's type: Ref<string | number>

foo.value = 123 // ok!
```

## isRef
检查值是否是引用对象。

```javascript
const unwrapped = isRef(foo) ? foo.value : foo
```
#### typing 类型

```typescript
function isRef(value: any): value is Ref<any>
```

## toRefs
将反应对象转换为普通对象，其中结果对象上的每个属性都是指向原始对象中相应属性的`ref`

```javascript
    const state = reactive({
      foo: 1,
      bar: 2
    })

    const stateAsRefs = toRefs(state)
    /*
    Type of stateAsRefs:

    {
      foo: Ref<1>,
      bar: Ref<2>
    }
    */

    // The ref and the original property is "linked"
    state.foo++
    console.log(stateAsRefs.foo) // 2

    stateAsRefs.foo.value++
    console.log(state.foo) // 3
```
`toRefs` 从组合函数返回反应对象时，此函数很有用，以便使用组件可以分解/散布返回的对象而不会失去反应性：

```javascript
    function useFeatureX() {
      const state = reactive({
        foo: 1,
        bar: 2
      })

      // logic operating on state

      // convert to refs when returning
      return toRefs(state)
    }

    export default {
      setup() {
        // can destructure without losing reactivity
        const { foo, bar } = useFeatureX()

        return {
          foo,
          bar
        }
      }
    }
```

## Computed
采用getter函数，并为getter返回的值返回一个不变的反应性ref对象。

```javascript
  const count = ref(1)
  const plusOne = computed(() => count.value + 1)

  console.log(plusOne.value) // 2

  plusOne.value++ // error
```
或者，它可以使用具有get和set功能的对象来创建可写的ref对象。

```javascript
    const count = ref(1)
    const plusOne = computed({
      get: () => count.value + 1,
      set: val => { count.value = val - 1 }
    })

    plusOne.value = 1
    console.log(count.value) // 0
```

#### typing 类型

```typescript
    // read-only
    function computed<T>(getter: () => T): Readonly<Ref<Readonly<T>>>

    // writable
    function computed<T>(options: {
      get: () => T,
      set: (value: T) => void
    }): Ref<T>
```


### 未完待续.......