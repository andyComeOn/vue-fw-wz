store中有一个index.js 文件，在modules中有几个文件，最后引入了getters.js。


const getters = {
  sidebar: state => state.app.sidebar,
  visitedViews: state => state.app.visitedViews,

  token: state => state.user.token,
  avatar: state => state.user.avatar,
  name: state => state.user.name,
  uid: state => state.user.uid,
  email: state => state.user.email,
  introduction: state => state.user.introduction,
  auth_type: state => state.user.auth_type,
  status: state => state.user.status,
  roles: state => state.user.roles,
  setting: state => state.user.setting,

  permission_routers: state => state.permission.routers,
  addRouters: state => state.permission.addRouters,
  siderbar_routers:state => state.permission.siderbar_routers,
};

既然是把getters提出来了，在modules中的几个js中都没有写getters了。

在官网上首先讲述的是state 

// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。

computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])

mapState 函数返回的是一个对象。我们如何将它与局部计算属性混合使用呢？
通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 computed 属性。但是自从有了对象展开运算符
（现处于 ECMAScript 提案 stage-4 阶段），我们可以极大地简化写法：

computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。
就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。




在modules中的permission.js中有很多的函数就会对权限进行判断。


在src/login.js中有对router的守卫，这是对路由的操作。

登录之后mock的响应：
introduction: "我是超级管理员"
name: "Super Admin"
role: ["admin"]
token: "admin"
uid: "001"

点击登录页面按钮dispatch('LoginByEmail),promise的loginByEmail（是mock的数据），response之后，开始
Cookies.set('Admin-Token', response.data.token);
commit('SET_TOKEN', data.token);
commit('SET_EMAIL', email);
resolve();

这个时候在src/login.js中就是一直在监听这路由，token已经种上，在该login.js中，就已经进入在进入path:'/'
时候就已经在走

if (store.getters.roles.length === 0) { // 判断当前用户是否已拉取完user_info信息
    store.dispatch('GetInfo').then(res => { // 拉取user_info
        const roles = res.data.role
        store.dispatch('GenerateRoutes', { roles }).then(() => { // 生成可访问的路由表
        router.addRoutes(store.getters.addRouters) // 动态添加可访问路由表
        next({ ...to }) // hack方法 确保addRoutes已完成
        })
    }).catch(() => {
        store.dispatch('FedLogOut').then(() => {
        next({ path: '/login' })
        })
    })
}

拉取user_info，上面代码是拉取了info把role带上又去store.dispatch('GenerateRoutes')


在浏览器的application中删除Cookie在此点击时候，就会马上甩到/login页


点击侧边栏的任何一个，路由都会监听



npm WARN deprecated browserslist@2.11.3: Browserslist 2 could fail on reading Browserslist >3.0 config used in other tools.
npm WARN deprecated browserslist@1.7.7: Browserslist 2 could fail on reading Browserslist >3.0 config used in other tools.
npm WARN deprecated circular-json@0.3.3: CircularJSON is in maintenance only, flatted is its successor.

browserslist的帖子
https://blog.csdn.net/qq_41157421/article/details/83016874

https://www.jianshu.com/p/d45a31c50711

https://www.jianshu.com/p/2a935a7dbab0
