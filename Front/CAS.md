整体流程设计

1. 浏览器访问子系统 A 前端页面，前端向 Client 获取用户信息
2. Client 发现请求中没有会话 ID，返回 401 及用于跳转页面的 Controller 的地址。
3. 前端发现 401，将浏览器重定向到 CAS Server 的登录页，后面带上 service 参数，service 即为 Client 回传的 Controller 地址，同时向其中传入一个 url 参数，用于返回前端页面
4. CAS Server 登录，登录成功后根据 service 参数返回 Client 中的 Controller
5. Client 接收 url 参数，同时将 JSESSIONID 拼接在 url 后，通过跳转回传给前端
6. 前端接收 JSESSIONID 并存放在其自身域的 cookie 中，后续请求均携带 cookie
7. 另一子系统 B 前端访问 Client2 时，Client2 发现无会话，同样返回 401
8. 前端跳转 CAS Server 进行登录认证（携带 service 及 url 两个参数），CAS 发现已登录，直接跳转回到 service 中
9. Client2 根据 url 跳转回前端 B，并在地址栏增加 JSESSIONID 参数
10. 前端 B 接收 JSESSIONID 并存放在 B 域的 cookie 中，后续请求均携带 cookie
