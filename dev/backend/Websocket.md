基于HTTP/2的 [Web API](Web%20API.md)协议，允许在客户端和服务器之间建立**持久连接**，实现实时的**双向**数据传输。
## 优缺点
### 优点

1. **实时性和低延迟**：WebSocket 允许客户端和服务器之间建立持久的连接，实时性更高，通信过程中的延迟较低，适用于需要实时数据更新的应用场景，如在线聊天、实时游戏等。

2. **双向通信**：WebSocket 支持全双工通信，客户端和服务器都可以随时发送和接收数据，而不需要等待请求和响应的周期。

3. **减少网络开销**：相对于一些轮询或长轮询技术，WebSocket 在保持连接的同时减少了不必要的网络开销和资源消耗。

4. **较少的头部开销**：与 HTTP 请求相比，WebSocket 的数据传输具有较少的头部开销，因为连接建立后，消息传输中的头部信息较少。

### 缺点

1. **不适用于所有情况**：对于一些仅需要单向通信或对实时性要求不高的应用，WebSocket 可能显得过于复杂和不必要。

2. **兼容性问题**：WebSocket 在某些旧版本的浏览器中可能不受支持，需要考虑兼容性问题。

3. **维护连接**：WebSocket 需要保持持久连接，这可能导致一些资源占用和管理上的挑战。特别是在大量连接的情况下，服务器端需要有效地管理这些连接。

4. **安全性问题**：与传统的 HTTP 请求不同，WebSocket 连接可能更容易受到某些安全漏洞的影响，需要谨慎处理安全性问题，如防止跨站脚本攻击（XSS）和保护连接的身份验证等。

总的来说，WebSocket API 提供了实时性强、双向通信的优势，特别适用于需要实时数据传输和实时更新的应用场景。然而，在考虑使用时，也需要权衡其优点和缺点，并根据具体的应用需求来选择合适的技术方案。