See also: [Platform Building Cheat Sheet][1]

# API 设计参考手册
1. 就像是做自己的产品一样，以资源消费者的角度去设计 API；
	* 不是为特定的用户界面设计 API；
	* 拥抱变化，针对每一个接口尽量满足灵活性和可调性。(参见 5，6 和 7)；
	* 吃自己的狗粮，即使你必须模拟一些示例界面。

2. 使用资源集合的概念；
	* 每种资源有两类网址（接口）：
		* 资源集合（例如，/orders）；
		* 集合中的单个资源（例如，/orders/{orderId}）。
	* 使用复数形式 (使用 ‘orders’ 而不是 ‘order’)；
	* 资源名称和 ID 组合可以作为一个网址的节点（例如，/orders/{orderId}/items/{itemId}）；
	* 尽可能的让网址越短越好，单个网址最好不超过三个节点。

3. 使用名词作为资源名称 (例如，不要在网址中使用动词)；

4. 使用有意义的资源描述；
	* “禁止单纯的使用 ID!” 响应信息中不应该存在单纯的 ID，应使用链接或是引用的对象；
	* 设计资源的描述信息，而不是简简单单的做数据库表的映射；
	* 合并描述信息，不要通过两个 ID 直接表示两个表的关系；

5. 资源的集合应支持过滤，排序和分页；

6. 支持通过链接扩展关系，允许客户端通过添加链接扩展响应中的数据；

7. 支持资源的字段裁剪，运行客户端减少响应中返回的字段数量；

8. 使用 HTTP 方法名来表示对应的行为：
	* POST - 创建资源，非幂等性操作；
	* PUT - 更新资源；
	* GET - 获取单个资源或资源集合；
	* DELETE - 删除单个资源或资源集合；

9. 合理使用 HTTP 状态码；
	* 200 - 成功；
	* 201 - 创建成功，成功创建一个新资源时返回。 返回信息中将包含一个 'Location' 报头，他通过一个链接指向新创建的资源地址；
	* 400 - 错误的请求，数据问题，如不正确的 JSON 等；
	* 404 - 未找到，通过 GET 请求未找到对应的资源；
	* 409 - 冲突，将出现重复的数据或是无效的数据状态。

10. 使用 ISO 8601 时间戳格式来表示日期；

11. 利用链接策略来处理关联关系，一些流行的例子如下：
	* [HAL][2]
	* [Siren][3]
	* [JSON-LD][4]
	* [Collection+JSON][5]

12. 使用 [OAuth2][6] 来确保 API 的安全性；
	* 使用 Bearer Token 进行身份验证；
	* OAuth2 的 Bearer token 要求必须通过 HTTPS / TLS / SSL 来访问你的 API。通过 HTTP 进行的未加密通信将导致窃听和重放。

13. Use Content-Type negotiation to describe incoming request payloads.

14. Evolution over versioning. However, if versioning, use the Accept header instead of versioning in the URL.

15. Consider Cache-ability. At a minimum, use the following response headers:

16. 确保你的 GET，PUT，DELETE 请求是[幂等的][7]，这些请求多次操作不应该有副作用。

[1]:	https://github.com/RestCheatSheet/platform-cheat-sheet#platform-building-cheat-sheet
[2]:	http://stateless.co/hal_specification.html
[3]:	https://github.com/kevinswiber/siren
[4]:	http://json-ld.org/
[5]:	http://amundsen.com/media-types/collection/
[6]:	http://oauth.net/2/
[7]:	http://www.restapitutorial.com/lessons/idempotency.html