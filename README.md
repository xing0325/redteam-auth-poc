# 授权绕过红队 PoC

目标页面：`https://www.xpcxl.com/lsyxss.html`

验证日期：2026-09-26

## 证据文件

- `original.html`：未经修改的公开 HTTP 响应。
- `index.html`：仅移除客户端授权包装后的本地 PoC。
- 原始响应 SHA-256：`3F75CC2E0038EF25157D1BD4C0D7181944926A71051358A9F61FE231BC794C3F`

## 最小修改

1. 将入口从 `checkAuthCode(startTest)` 改为 `startTest()`。
2. 删除 `/js/common.js`、jQuery、Layer 三项授权弹窗依赖。
3. 删除 `checkAuthCode`、兑换码 Cookie 和授权接口调用代码。

差异总计：1 行新增，78 行删除。

## 验证结果

- 本地页面返回 HTTP 200。
- 未设置 `HhcsAuthCode` 或 `UserIdentity`。
- 未调用 `/HhcsAuthCode/CheckHhcsAuthCode`。
- 可正常进入第 1 题。
- 可完成全部 32 题。
- 可在纯前端生成完整人物报告、维度分数、证据、建议和相似人物。

## 本地运行

在此目录执行：

```powershell
python -m http.server 8765 --bind 127.0.0.1
```

然后打开 `http://127.0.0.1:8765/`。

## 根因

服务器在授权前已把题库、选项权重、人物数据、报告文案和计算算法全部发送给浏览器。授权仅控制正常 UI 是否调用 `startTest()`，没有保护核心资产。
