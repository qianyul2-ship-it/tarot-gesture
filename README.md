# Tarot · Sakura AI · Ritual Edition

Sakura 视觉风格的手势塔罗网页，保留原项目全部能力：

- 手掌左右移动控制 3D 卡牌转盘
- 握拳抓取当前卡牌
- 🤘 手势切换 1 / 3 / 5 张牌阵
- 无摄像头时支持按钮抽牌
- MediaPipe 浏览器本地手势识别，摄像头画面不上传
- 完整 78 张塔罗数据与正逆位基础牌义
- DeepSeek AI 根据问题、牌阵位置和牌面关系生成个性化解读
- “输入问题 → 选择牌阵 → 牌组展开 → 抽牌 → AI 解读”的分镜式引导流程
- 78 张经典 Rider–Waite 牌面图片与 Sakura 粉色卡框
- 揭晓后松开手掌不会再清空结果，AI 按钮会保持显示
- 首次点击允许摄像头后，支持全程手势控制
- 初次 AI 解读后支持最多 3 次按钮追问
- 右上角“重新抽卡”在所有步骤常驻

## 全程手势

浏览器基于隐私规则，第一次仍需要用户点击“开启全程手势”并允许摄像头。授权后可以免触控完成完整流程：

- 问题步骤：✌ 胜利手势跳过输入，选择开放式探索
- 牌阵步骤：🤘 依次切换 1 / 3 / 5 张牌阵，✊ 握拳确认
- 牌组展开：握拳确认牌阵后自动完成，约 1 秒即可抽牌
- 抽牌步骤：🖐 左右移动卡牌，✊ 握拳抓取
- 结果步骤：👍 点赞请求 DeepSeek AI 深度解读
- 重新开始：✌ 保持胜利手势

追问只使用页面按钮提交，不占用任何手势；每轮最多追问 3 次。

页面底部会随流程显示当前可用手势及确认进度。

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

## 牌面素材

项目内 78 张 Rider–Waite 图片来自
[`@cometpisces/tarot-kit-images`](https://www.npmjs.com/package/@cometpisces/tarot-kit-images)。
该包将代码与映射以 MIT 许可证发布，并说明其中 Rider–Waite 原始牌面因 1909 年首次出版而在许多司法辖区被视为公版。

具体说明见 `ASSET_LICENSES.md`。如用于商业用途，应根据运营所在地再次确认公版状态；不要将这些图片误认为现代商业粉色套牌的授权素材。
