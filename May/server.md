# 服务端
服务端使用的是Django框架。主要有登录验证、跨域处理、rest风格api。
## 1. 登录验证(djangorestframework-jwt)
登录验证往往伴随着用户权限的配置。在这里，以get不需要携带token而post/put/delete需要携带token为例。那么需要以下几个步骤。
1. 登录获取token,保存至本地客户端。
2. get请求不携带token，其他请求从客户端获取token。
### 客户端请求拦截器(axios为例)
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
## 跨域/cors(django-cors-headers)
跨站HTTP请求就是从另一个域名，而不是资源所在的域名发起的 HTTP请求。即协议、域名、端口不一致的请求。同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。
### 后端配置(Access-Control-Allow-Origin)
```
CORS_ORIGIN_WHITELIST = (
  // host
)
```
## HTTP响应码
### 成功(2xx)
1. 200 // ok
2. 204 // no content
3. 206 // Partial Content(已成功处理部分请求)

### 重定向(3xx)
1. 301 // Moved Permanently
2. 302 // Found

### 客户端响应(4xx)
1. 400 // Bad Request
2. 401 // Unauthorized
3. 403 // Forbidden
4. 404 // Not Found

### 服务端响应(5xx)
1. 500 // Internal Server Error
2. 502 // Bad Gateway
3. 503 // Service Unavailable