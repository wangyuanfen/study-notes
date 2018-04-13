### vue生命周期描述
beforecreated：el和data并未初始化   
created:完成了 data数据的初始化，el没有  
beforeMount：完成了el和data初始化   
mounted ：完成挂载  
### 父子组件通信
props 

使用 $on(eventName) 监听事件  
使用 $emit(eventName, optionalPayload) 触发事件  

.sync修饰符
### 非父子组件通信

  var bus = new Vue()
  // 触发组件 A 中的事件
  bus.$emit('id-selected', 1)
  // 在组件 B 创建的钩子中监听事件
  bus.$on('id-selected', function (id) {
    // ...
  })
  
vuex状态管理
### vue实例化后往data添加变量是否会触发视图更新
不会，只有当实例被创建时 data 中存在的属性才是响应式的，变换才会触发视图更新
### compted和function
计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。  
调用方法将总会再次执行函数。 
### computed和watch 
当需要在数据变化时执行异步或开销较大的操作。例如，调用function进行数据异步获取等，使用watch。
### vue数组、对象注意事项
由于 JavaScript 的限制，Vue 不能检测以下变动的数组：  
当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue  
当你修改数组的长度时，例如：vm.items.length = newLength  
Vue 不能检测对象属性的添加或删除
