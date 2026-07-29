# Tarot · Sakura AI

Sakura 视觉风格的手势塔罗网页，保留原项目全部能力：

- 手掌左右移动控制 3D 卡牌转盘
- 握拳抓取当前卡牌
- 胜利手势切换 1 / 3 / 5 张牌阵
- 无摄像头时支持按钮抽牌
- MediaPipe 浏览器本地手势识别，摄像头画面不上传
- 完整 78 张塔罗数据与正逆位基础牌义
- DeepSeek AI 根据问题、牌阵位置和牌面关系生成个性化解读

## 本地预览

```bash
python3 -m http.server 8765
```

打开 `http://localhost:8765`。普通静态服务器可以测试抽牌和手势，但 AI 接口需要 EdgeOne Pages Functions 或 Cloudflare Pages Functions。

## EdgeOne Pages 部署（推荐）

1. 将整个项目推送到你 Fork 后的 GitHub 仓库。
2. 在 EdgeOne Pages 创建项目并连接该仓库。
3. 这是纯静态项目，不需要构建命令；发布目录选择项目根目录。
4. 在项目环境变量中新增 Secret：

   ```text
   DEEPSEEK_API_KEY=你的真实 DeepSeek API Key
   ```

5. 重新部署。`functions/api/reading.js` 会自动形成 `/api/reading` 接口。

## Cloudflare Pages

连接同一个 GitHub 仓库并部署；在 Pages 项目的 Settings → Variables and Secrets 中加入 `DEEPSEEK_API_KEY`。当前函数采用与 EdgeOne Pages / Cloudflare Pages 兼容的 `onRequestPost` 格式。

## GitHub Pages

GitHub Pages 只能部署静态内容，不能安全保存 API Key，所以抽牌与手势功能可正常使用，但 AI 解读需要调用已经部署在 EdgeOne 上的接口。

部署 EdgeOne 后，把 `index.html` 的 `<body>` 改为：

```html
<body data-theme="sakura" data-ai-endpoint="https://你的EdgeOne域名/api/reading">
```

这样 GitHub Pages 版本也会使用 EdgeOne 的 AI 接口。跨域使用前应在函数中仅为你自己的 GitHub Pages 域名添加 CORS 响应头。

## 安全

- 不要把 DeepSeek API Key 写入 `index.html`、提交到 GitHub 或发送给其他人。
- API Key 必须配置为部署平台的 Secret。
- 正式开放前建议在平台侧开启请求频率限制和每日费用上限。
- AI 解读仅供娱乐与自我反思，不构成专业建议。

