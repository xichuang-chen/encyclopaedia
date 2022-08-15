#Http

## Http 缓存
[请详细查看 Http 缓存](https://blog.csdn.net/HHXX360TTXS361/article/details/110877081)
- 强缓存(cache-control)
- 对比缓存(Etag | Last-Modify)
- cache-control 是由服务端返回，在 response header 的头里, 取值有：max-age, no-cache, no-store, private, public
- 有了 Etag 为何还要用 last-modify ？
- 命中强缓存返回 200, size 列显示 from cache
- 命中对比缓存是 304

## 从在浏览器中输入 url 到看到页面发生了什么
[请详看 http 笔记](https://blog.csdn.net/HHXX360TTXS361/article/details/110877081)