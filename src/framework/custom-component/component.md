# Component 构造器

Component 构造器可用于定义组件，调用 Component 构造器时可以指定组件的属性、数据、方法等。

```js
{
  /** 组件的对外属性 */
  properties: {
    /** 属性类型
    * 目前接受的类型包括：
    * String, Number, Boolean, Object, Array, null（表示任意类型）
    */
    type: String,
    /** 属性初始值 */
    value: 'default value',
    /** 属性值改变时的响应函数 */
    observer: function(newVal, oldVal){},
  },
  /** 组件的内部数据 */
  data: {},
  /** 组件的方法，包括事件响应函数和任意的自定义方法 */
  methods: {
    customMethod: function(){}
  },
  /** 类似于 mixins 和 traits 的组件间代码复用机制 */
  behaviors: [],
  /**
  * 组件生命周期函数，在组件实例进入页面节点树时执行，
  * 注意此时不能调用 `setData`
  */
  created: function() {},
  /**
  * 组件生命周期函数，在组件实例进入页面节点树时执行
  */
  attached: function() {},
  /** 组件生命周期函数，在组件布局完成后执行，
  * 此时可以获取节点信息（使用 SelectorQuery ）
  */
  ready: function() {},
  /** 
  * 组件生命周期函数，在组件实例被移动到节点树另一个位置时执行
  */
  moved: function() {},
  /**
  * 组件生命周期函数，在组件实例被从页面节点树移除时执行
  */
  detached: function() {},
  /** 组件间关系定义 */
  relations: {},
  /** 一些组件选项 */
  options: {}
}
```

生成的组件实例可以在组件的方法、生命周期函数和属性 `observer` 中通过 `this` 访问。组件包含一些通用属性和方法。

```js
/** 属性名 */

{
  is: '组件的文件路径',
  id: '节点id',
  dataset: '节点dataset',
  /** 组件数据，包括内部数据和属性值 */
  data: {}
}

/** 方法名 */

/** 设置 data 并执行视图层渲染 */
setData(newData: Object)

/** 检查组件是否具有 behavior 
*（检查时会递归检查被直接或间接引入的所有 behavior）
*/
hasBehavior(behavior)

/** 触发事件 */
triggerEvent(name: String, detail: Object, options: Object)

/** 创建一个 SelectorQuery 对象，选择器选取范围为这个组件实例内 */
createSelectorQuery()

/** 使用选择器选择组件实例节点，返回匹配到的第一个组件实例对象 */
selectComponent(selector: String)

/** 使用选择器选择组件实例节点，返回匹配到的全部组件实例对象组成的数组 */
selectAllComponents(selector: String)

/** 获取所有这个关系对应的所有关联节点 */
getRelationNodes(relationKey: String)
```

**代码示例：**

```js
Component({
  behaviors: [],
  properties: {
    myProperty: {
      type: String,
      value: '',
      observer: function(newVal, oldVal){}
    },
    myProperty2: String // 简化的定义方式
  },
  data: {}, // 私有数据，可用于模版渲染

  // 生命周期函数，可以为函数，或一个在 methods 段中定义的方法名
  attached: function(){},
  moved: function(){},
  detached: function(){},

  methods: {
    onMyButtonTap: function(){
      this.setData({
        // 更新属性和数据的方法与更新页面数据的方法类似
      })
    },

    _myPrivateMethod: function(){
      // 内部方法建议以下划线开头
      this.replaceDataOnPath(['A', 0, 'B'], 'myPrivateData') // 这里将 data.A[0].B 设为 'myPrivateData'
      this.applyDataUpdates()
    },

    _propertyChange: function(newVal, oldVal) {}
  }
})
```

注意：在 `properties` 定义段中，属性名采用驼峰写法（`propertyName`）；在 wxml 中，指定属性值时则对应使用连字符写法（`component-tag-name property-name="attr value"`），应用于数据绑定时采用驼峰写法（`attr="{{propertyName}}"`）。

## 小贴士

- Component 构造器构造的组件也可以作为页面使用。
- 使用 `this.data` 可以获取内部数据和属性值，但不要直接修改它们，应使用 `setData` 修改。
- 生命周期函数无法在组件方法中通过 `this` 访问到。
- 属性名应避免以 `data` 开头，即不要命名成 `dataXyz` 这样的形式，因为在 WXML 中， `data-xyz=""` 会被作为节点 `dataset` 来处理，而不是组件属性。
- 在一个组件的定义和使用时，组件的属性名和 data 字段相互间都不能冲突（尽管它们位于不同的定义段中）。

## REF

- [Component 构造器 - 小程序][doc]

[doc]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/component.html