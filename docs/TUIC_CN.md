# TUIC 出口接入

Sub2API 原生代理字段只接受 `http`、`https`、`socks5` 和 `socks5h`。TUIC 应作为出口传输层接入，不应把 `tuic://` URL 直接填入 Sub2API。

```text
Sub2API -> socks5h://sing-box:1080 -> sing-box TUIC client -> TUIC server -> AI upstream
```

这种边界让 Sub2API 继续使用已经测试过的 HTTP/SSE/WebSocket 路径，由 sing-box 单独管理 QUIC、UDP、拥塞控制和 TUIC 会话。TUIC 节点异常时也可以单独停用，不影响未绑定该代理的账号。

## 启用

1. 准备一台允许 UDP 出站的 Sub2API 服务器，以及一个可用的 TUIC v5 服务端。
2. 在 `deploy` 目录复制示例配置：

   ```bash
   cp tuic/sing-box.example.json tuic/config.json
   chmod 600 tuic/config.json
   ```

3. 修改 `tuic/config.json`：
   - SOCKS 用户密码；
   - TUIC 服务端域名和端口；
   - UUID 和密码；
   - TLS `server_name`。
4. 先检查配置，再启动：

   ```bash
   docker run --rm -v "$PWD/tuic/config.json:/etc/sing-box/config.json:ro" \
     ghcr.io/sagernet/sing-box:v1.12.15 check -c /etc/sing-box/config.json

   docker compose -f docker-compose.yml -f docker-compose.tuic.yml \
     --profile tuic up -d
   ```

5. 在 Sub2API 管理后台的 IP 管理中添加：
   - 协议：`socks5h`
   - 主机：`sing-box`
   - 端口：`1080`
   - 用户名：`sub2api`
   - 密码：配置中的 SOCKS 密码
6. 先绑定一个测试账号，验证普通请求、SSE 流式响应、WebSocket、额度查询和 OAuth 刷新，再逐步扩大流量。

## 安全与稳定性

- Compose 不发布 `1080` 到宿主机，SOCKS 只允许同一 Docker 网络访问。
- `deploy/tuic/config.json`、证书和私钥被 Git 忽略；不要在 Issue、日志或截图中暴露它们。
- 生产环境固定 sing-box 镜像版本和摘要，升级前先在测试机验证。
- 保持 `zero_rtt_handshake` 关闭，除非已经评估重放风险并确认服务端配置匹配。
- TUIC 更适合网络丢包、跨境链路或需要固定出口的特定账号，不应默认承载全部流量。
- 监控 TUIC 握手失败、RTT、丢包、重连次数、首 Token 时间和流式中断率。HTTP 请求一旦已经向客户端输出内容，不做透明重放。

参考：

- [TUIC protocol](https://github.com/tuic-protocol/tuic)
- [sing-box TUIC outbound](https://sing-box.sagernet.org/configuration/outbound/tuic/)
