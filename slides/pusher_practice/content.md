## 一起长大推送系统实践

---

## 分享三个部分

<p class="fragment">个推的接入 <span class="fragment">=> 其实就是介绍下个推 API</span></p>

<p class="fragment">介绍一下开发的过程 <span class="fragment">=> 只是一些个人的实践 </span></p>

<p class="fragment">异步消息模式介绍 <span class="fragment">=> 我们其实没用上, 凑时间的</span></p>

----

## 个推的接入

---

### 平台注册

会注册一个 App 唯一的 app id.
<!-- .element: class="fragment" -->

会注册一个 App 与 SDK 通信的 app key.
<!-- .element: class="fragment" -->

会生成一个可以反复重置的 master secret.
<!-- .element: class="fragment" -->

---

### 发送推送

发送 RESTFul 请求, 申请一个 auth_token.
 <!-- .element: class="fragment" -->

HEADER 里面带上这个 token, 再发 RESTFul 请求.
 <!-- .element: class="fragment" -->

<span style="color: red">OVER</span>
<!-- .element: class="fragment" -->

---

举个例子:

```sh

app_id='我的app_id'  # => u2d3dqsNP78chrHYXsNn47
app_key='我的app_key' # => KgcnFbGiun5jUb9hgejc28
master_secret='我的master_secret' # => uHb0H36NbUAScEiaHGfshA

timestamp=$(($(date +%s)*1000)) # => 1542644939000

sign=$(echo -n "${app_key}${timestamp}${master_secret}" |sha256sum |cut -d' ' -f1)

curl -s -H "Content-Type: application/json" \
    https://restapi.getui.com/v1/${app_id}/auth_sign \
    -XPOST -d "{
        \"sign\":\"${sign}\",
        \"timestamp\":\"${timestamp}\",
    \"appkey\":\"${app_key}\"
}"
```
---

```sh

curl -s -H Content-Type: application/json \
    https://restapi.getui.com/v1/u2d3dqsNP78chrHYXsNn47/auth_sign
    -XPOST -d {
        "sign":"f9e069653c08bc35b9e04921922a44459cf7d530e98fc281df3f83ef8e9457f5",
        "timestamp":"1542645250000",
        "appkey":"KgcnFbGiun5jUb9hgejc28"
    }

```

curl 命令:

 <small><span style="color: yellow">-s, --silent</span>        # => 静默模式, 不显示 "进度条" 以及 "错误信息"</small>
<!-- .slide: style="text-align: left;" -->

<small><span style="color: yellow">-H, --header</span>        # => 指定一个 header, 多个 header 可以指定多个 -H</small>
<!-- .slide: style="text-align: left;" -->

<small><span style="color: yellow">-X, --request</span>       # => 指定请求动词</small>
<!-- .slide: style="text-align: left;" -->

<small><span style="color: yellow">-d, --data</span>          # => 如果是 POST 请求, 后面跟一个字符串, 作为 data</small>
<!-- .slide: style="text-align: left;" -->

---

```json
{
    "result":"ok",
    "expire_time":"1542731631319",
    "auth_token":"0ea73c350088b911f85c9b0673b22afbdc6c645ce7cb1cfef71c51a8eca9bd92"
}
```

---

发送推送

```sh
curl -H Content-Type: application/json \
    -H authtoken:1bc67992a3a4ffd4a4c43cc836646aa4ee9b4334455198bc4a7cf2bd180a8ee7 \
    https://restapi.getui.com/v1/u2d3dqsNP78chrHYXsNn47/push_single \
    -XPOST -d {
        "message": {
        "appkey": "KgcnFbGiun5jUb9hgejc28",
        "is_offline": true,
        "offline_expire_time":10000000,
        "push_network_type":0,
        "msgtype": "notification"
        },
        "notification": {
        "style": {
            "type": 0,
            "text": "默认通知内容",
            "title": "默认通知标题"
            },
        "transmission_type": true,
        "transmission_content": "默认透传内容"
        },
        "cid": "ecc39e7d87cf4db40dcac8d92ca6e127",
        "requestid": "a74d-4c27-93a4-2ff96dd7d885"
    }
```

<small>按照要求的格式, 拼好要发送的 json.</small>
<!-- .element: class="fragment" -->

<small>客户端 SDK 收到消息, 自己决定如何处理.</small>
<!-- .element: class="fragment" -->

---

### 几个需要注意的点:

<small><span style="color: yellow">-H authtoken: </span>之前请求的 auth_token, 它一段时间才会过期, 你不需要总是请求它.</small>

<small><span style="color: yellow">cid: </span>每个设备唯一的 client id</small>

<small><span style="color: yellow">requestid: </span>按照个推的要求, 生成一个 30 位的 requestid 给它. 我们后面会提到它</span></small>

---

### 开发中遇到的一些坑

 <small>1. Header 的 key 大小写敏感, 这是不符合 HTTP 规范的.</small>
<!-- .element: class="fragment" -->
<!-- .slide: style="text-align: left;" -->

<small>2. 文档分散, 不一致, 例如: 获取 auto_token 的时候, sign 使用 sha256 还是 sha128</small>
<!-- .element: class="fragment" -->
<!-- .slide: style="text-align: left;" -->

<small>3. 讨论时需求搞的很复杂, 个推所有接口都 wrap 了一遍, 但用的时候, 除了<span style="color:red"> "单推" </span>, 没人 care 了,
即使很多内容完全一致的推送, 可以几乎不做更改的替换成<span style="color:red"> "群推" </span>接口.</small>
<!-- .element: class="fragment" -->
<!-- .slide: style="text-align: left;" -->

---

### 推送讲完了

----

## 简单介绍下开发的过程

---

先写了几个 Bash shell 脚本, 做实验

=> 看代码
<!-- .element: class="fragment" -->

还有个额外的好处:
<!-- .element: class="fragment" -->

<p class="fragment"> 用这些脚本调试个推的服务<span class="fragment">. => 搞清楚谁的锅</span></p>

---

项目本身的特点, `足够轻量`, `快速`.

Rails 过于庞大, 复杂, Overkill.
<!-- .element: class="fragment" -->

---

减少 Rails 默认组件的使用

=> 看代码 (Gemfile, config/appliation.rb)
<!-- .element: class="fragment" -->

---

编写不依赖于 Rails 的 Ruby 代码.

几乎所有业务代码, 都可以不加改动, 在其他框架中使用. (例如 Roda)
<!-- .element: class="fragment" -->

=> 看代码 (/lib)
<!-- .element: class="fragment" -->


---

### 一些个人实践

先使用最小代码验证, 例如: bash shell.
<!-- .element: class="fragment" -->

编写测试, 并且使用 VCR, 不断重构代码.
<!-- .element: class="fragment" -->

调用 "外部服务" 的代码, 封装成库, 放在 lib/ 下面.
<!-- .element: class="fragment" -->

保存第三方服务返回的 response 到一个 json 字段. (你不知道将来那些信息有用)
<!-- .element: class="fragment" -->

作为 API 给别的项目调用时, 方法使用 named parameter (命名参数), 含义更清晰.
<!-- .element: class="fragment" -->

---

遇到的问题:

<small>1. 个推的推送任务, 是异步执行的, 只是返回一个 taskid</small>
 <!-- .element: class="fragment" -->
<!-- .slide: style="text-align: left;" -->

```json
{
'taskid' => 'RASS_0817_d6708f78a2e08221ae13d17b05726f48',
'status' => 'successed_offline'
}
```
<!-- .element: class="fragment" -->

<small class="fragment">2. 返回 response 通常比较快, 通常不需要等待太久(一秒以内)<span style="color:red" class="fragment fade-right">, 但还是要等的!<span></small>

发送请求应该使用异步.
<!-- .element: class="fragment" -->

----

## 异步消息模式介绍

---

- 不要一开始就考虑异步, 首先功能正确, 再换异步.

- 为异步代码添加测试 (异步的库一般都提供插件)

- 只在需要的时候才加.

---

### <span style="color: red">"业务模式"</span> 决定采用的异步模型

---

pusher 业务模式:

<small>1. 个推只是创建 task, 并返回 taskid, 通常比较快, 几乎没有阻滞的风险.</small>
<small>2. 失败的可能性比较小, 没有<span style="color: red"> 重新再发 </span>的需求.</small>
<small>2. pusher 属于非关键性业务, 偶尔丢几条也没关系.</small>

典型的 fast and non-mission critical.

---

结论: 使用 <span style="color: red">单一内存队列</span> 就足够了.

=> 看代码 app/controllers/pushers_controller.rb#push_single_async
<!-- .element: class="fragment" -->

---

### 其他更复杂的模式

---

想象我们的队列中, 有多个不同目标的服务.

有的服务执行只需要 100ms, 有的则需要 10 秒.

<small>举例: 上传视频服务(10秒) 和 推送(100ms) 共用同一个队列, 上传视频如果失败, 还会反复重试, 引起队列阻塞.</small>



---

针对不同的服务, 使用单独的队列. <small>(queue per destination)</small>

视频和推送使用单独的队列.
<!-- .element: class="fragment" -->

并且考虑不同队列的优先级、吞吐量
<!-- .element: class="fragment" -->

<span style="color: red">问题解决</span>
<!-- .element: class="fragment" -->

---

### 还有更复杂的情况

多个消费者请求同一个服务
<!-- .element: class="fragment" -->

---

假设三个客户端请求推送服务:

第一个客户端要推送 10000 条, 需要 15 分钟
<!-- .element: class="fragment" -->

第二个客户端要推送 10 条, 需要 1 秒.
<!-- .element: class="fragment" -->

第三个客户端要推送 5 条, 需要 0.5 秒.
<!-- .element: class="fragment" -->

---

解决办法:

<p class="fragment"> 1. 等待第一个推完, 后面两个要等 15 分钟 <span class="fragment" style="color: red">不好.</span></p>
<p class="fragment"> 2. 将第一个拆分, 先推 100 条, 将剩下的 4900 条放入一个单独的延迟队列, 后面的先执行. <span class="fragment" style="color: red">复杂, 且增加了网络带宽、存储消耗.</span></p>

<p class="fragment">我们需要为 "<span style="color: green">每客户端/推送</span>" 创建一个单独队列</p>

---

仍然可能存在问题:

我们可能需要很多这样的 queue.
<!-- .element: class="fragment" -->
<small class="fragment">(想象我们是 "个推", 需要和 "一起长大" 建立一个 queue)</small>

可以希望动态调整某个队列的吞吐量.
<!-- .element: class="fragment" -->
<small class="fragment">(想象家长端有活动, 推送特别多)</small>

希望队列中的任务按照某些策略, 重新排序.
<!-- .element: class="fragment" -->
<small class="fragment">(按照优先级排序, 或消耗的时间排序)</small>

---

我们需要将 Queue 使用数据库存储.<small>database as a queue</small>

这个我还没细研究, 完全可以作为一个新的分享.
<!-- .element: class="fragment" -->

----

推送系统需要完善的地方:

使用 Roda 替换掉 Rails.
<!-- .element: class="fragment" -->

---

### Motive

- Rails 太复杂了，实现代码各种 `奇技淫巧`, 查看源码效率不高。

- 动态路由, 这是 Roda 区别于任何其他框架(rails, sinatra, grape等)的地方.

- plugins 系统, 所有的功能都是独立的插件，包括插件系统自己，我们不用不必要的插件, 
也可以自己编写项目特定目的插件. <span class="fragment" style="color:green">Sequel 就是这样做的</span>

---

<small>
ORM => 我们用的 Sequel, 最大的一个部分已经完成了.
<!-- .element: class="fragment" -->

自动化测试框架 => Ruby自带, 稍微 Hack 一下, 已经工作了
<!-- .element: class="fragment" -->

迁移代码 => 几乎不需要更改, 直接可用
<!-- .element: class="fragment" -->

功能验证 => 测试覆盖率: 97.74%
<!-- .element: class="fragment" -->

<p class="fragment">开发自动 reloader => [autoload_reloader](https://github.com/Shopify/autoload_reloader)</p>

<p class="fragment">数据库迁移 => [standalone_migrations](https://github.com/thuss/standalone-migrations)</p>

<p class="fragment" style="color: red">其他用到的库, 完全一样。</p>
</small>

---

要解决的问题:

1. 项目采用的目录结构.
2. 可能存在的安全风险.
3. 非常多的 [plugins](http://roda.jeremyevans.net/rdoc/classes/Roda/RodaPlugins.html), 该用哪个需要学习成本.

----

以前的一片 TDD 分享:

https://tech.kid17.com/tech-share/slides/pusher_practice/#/

---

Thanks

