# Vue
这应该是最难写的一篇文章了。虽然在之前的实习以及业余时间都用vue写过项目。但对web前端开发最系统的了解确是通过Angular，在我看来Vue + Vuex + Vue-Router = new FlexibleAngular().Angular过于繁琐，而Vue则恰恰相反，很灵活，因此我很喜欢在小项目、个人项目中使用。
## 生命周期
我不知道每次面试时都需要解释生命周期，因为我认为生命周期钩子函数只不过是在适当的时侯做了适当的事情。从数据出发，需要做的便是获取、验证、分发、更新及销毁。从这一角度讲，created适合做数据验证前后的事儿，而mounted则是对数据进行分发的控制。<br>
这些钩子函数的出现，大概是赋予了Vue实例兼容其他js插件的能力。下面是对生命周期的简单解释。
```
initLifecycle(vm)
initEvents(vm)
initRender(vm)
callHook(vm, 'beforeCreate')
initInjections(vm) // resolve injections before data/props
initState(vm)
initProvide(vm) // resolve provide after data/props
callHook(vm, 'created')

if (vm.$options.el) {
  vm.$mount(vm.$options.el)
}

callHook(vm, 'beforeMount')
if (vm.$vnode == null) {
  vm._isMounted = true
  callHook(vm, 'mounted')
}
```
### beforeCreated
beforeCreated前会调用initLifecycle、initEvents及initRender，这里做的是初始化实例中的某些参数。文件夹src/core/instance。
### created
created前会调用initInjections、initState及initProvide，这里做的主要是将props/data/computed/watcher/转化为Observable及将methods中的函数植入实例中。
### beforeMount
beforeMount前后判断是否有挂载的节点比较节点是否有变化。
### mounted
mounted前会创建$el，并挂载。
### update
当数据变化，调用beforeUpdate,然后虚拟DOM patch之后render()，再调用updated。
### destory
vm.$destory() => beforeDestory => 移除事件、子件、依赖等 => destory。

## vue-router
### 自动生成导航
```
function createNav(router) {
  // judge args of router
  for (let item in router) {
    doSomething(item)
    if (router.children && router.children.length > 0) {
      dosomething(item.children)
    }
  }
  }
  return something
}
createNav($router.options.routes)
```
### 路由守卫(beforeEnter)
demo: 忽略需要传递的信息
```
const userPromise = new Promise((resolve, reject) => {
  if (getToken()) {
    verfityToken()
      .then(() => resolve())
      .catch(() => reject())
  } else {
    reject()
  }
})
const userGuard = (to, from, next) => {
  userPromise()
    .then(() => next())
    .catch(() => next('/))
}
```
Promise: js引擎(v8)调用其他模块的接口后, 接收信息后处理信息的通用容器。例如，委托网络发送http请求，网络将返回的信息交给js引擎，js引擎将信息交给Promise

## vuex
### state
定义数据对象
### mutations
用于改变state
### actions
调用mutations中的函数。
demo:
```
const state = {
  token: getToken()
}
const mutations = {
  LOGIN(state, token) {
    state.token = token
  }
}
const actions = {
  login( { commit }, userInfo) {
    const { username = '', password = '' } = userInfo
    return new Promise((resolve, reject) => {
      login(username, password)
        .then(res => {
          const token = res.data.token
          if (token) {
            commit('LOGIN', token)
            saveToken(token)
          }
          resolve(token)
        }).catch(error => reject(error))
    })
  }
}
export default {
  namespaced: true,
  state,
  mutations,
  actions
}
```
### 组件内调用
```
import { mapGetters, mapActions } from 'vuex'
export default {
  name: 'VuexDemo',
  computed: {
    ...mapGetters(['user'])
  },
  methods: {
    ...mapActions({
      login: 'User/login'
    }),
    handleLogin() {
      // username, password
      this.login(username, password)
        .then()
        .catch()
    }
  }
}
```

## axios
### service
```
cosnt service = axios.create({
  baseURL: '',
  timeout: 5000
})
```
### interceptors
1. request
```
service.interceptors.request.use(
  config => {
    if (config.method === 'get' || config.permit) {
      return config
    }
    const token = getToken()
    if (token) {
      config.headers['Authorization'] = `JWT ${token}`
    }
    return config
  },
  error => {
    return Promise.reject(error)
  }
)
```
2. response
```
service.interceptors.response.use(
  response => response,
  error => {
    return Promise.reject(error)
  }
)
```
### api
```
function login(username, password) {
  return service({
    url: 'api-token-auth/',
    method: 'post',
    data: {
      username,
      password
    },
    permit: true //无需添加token
  })
}
```

