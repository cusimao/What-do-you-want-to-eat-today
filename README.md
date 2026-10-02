今天吃什么 🍚
一个单文件、完全离线的手机网页应用，用来记录每天吃了什么、算出热量缺口，并估算理论上的体重变化。
没有后端、没有账号、没有 CDN、没有统计代码 —— 所有数据都只存在你自己手机的浏览器里。
> 直接把 `index.html` 下载到手机打开就能用；也可以访问 **https://cusimao.github.io/What-do-you-want-to-eat-today/** ，然后在手机上「添加到主屏幕」当 App 用。
---
功能
1. 身体数据与每日消耗（TDEE）
输入性别、年龄、身高、体重与活动系数（久坐 1.2 / 轻度 1.4 / 中度 1.6 / 高度 1.8 / 极高 2.0）。
用 Mifflin-St Jeor 公式算基础代谢：
男：`10×体重 + 6.25×身高 − 5×年龄 + 5`
女：`10×体重 + 6.25×身高 − 5×年龄 − 161`
`TDEE = BMR × 活动系数`。身体数据只需填一次，之后自动带出。
2. 食物记录
输入中文食物名即时模糊搜索（最多 10 条），点一下即选中。
输入克数，自动按 `每 100g 热量 × 克数 ÷ 100` 算出这份的热量，并实时预览算式。
搜索不到时可手动输入食物名与每 100g 热量，存进「我的食物库」，下次直接搜得到。
每条记录显示食物名、克数、热量与时间，可单条删除或整天清空。
3. 内置食物热量数据库
415 种中国常见食物，每 100g 热量（kcal），参考《中国食物成分表》估算。
覆盖 11 个分类：主食 74、肉类 69、蔬菜 66、水果 40、零食 32、调味 29、饮料 28、快餐 25、蛋奶 21、豆制品 16、坚果 15。
支持分类浏览，也内置了常见别名（土豆/马铃薯、西红柿/番茄、猕猴桃/奇异果…）。
4. 日期系统
顶部可切换任意日期：`‹ 前一天` / `今天` / `后一天 ›`，或直接点「今天 / 昨天 / 前天 / 选日期…」。
底部固定汇总栏始终跟随所选日期，显示当日摄入 / 消耗 TDEE / 热量缺口（缺口为正显示绿色，为负显示红色）。
可以给过去的日子补记（按钮会提示「添加到 昨天（10月1日）」），未来日期会被拒绝。
每天的记录独立保存，第二天自动从空白开始；身体数据保留。
5. 趋势统计与理论体重变化
近 7 天 / 近 30 天切换，显示记录天数、平均摄入、平均缺口、累计缺口。
理论体重变化 = 累计热量缺口 ÷ 7700 kcal（约等于 1kg 脂肪）。缺口为正显示绿色「▼ 减重」，盈余显示红色「▲ 增重」。
柱状图逐日对比摄入量：超标变红、选中高亮、无记录为浅灰，虚线是 TDEE 参考线；点柱子直接跳到那一天。
只统计有记录的天数，避免「没记 = 零摄入」把结论算歪。
6. 手机体验
白底卡片式布局、圆角、蓝色主色，字体跟随系统，输入框 16px（避免 iOS 自动放大），适配刘海屏安全区。
离线可用，可「添加到主屏幕」独立窗口运行。
---
快速开始
方式一：直接当文件用（最省事）
下载 `index.html`。
用手机浏览器打开该文件，或先传到手机再用浏览器打开。
完全离线，飞行模式也能记录。
方式二：在线使用（已部署到 GitHub Pages）
手机上直接打开：https://cusimao.github.io/What-do-you-want-to-eat-today/
打开后 iPhone：Safari 分享 → 添加到主屏幕；Android：Chrome 菜单 → 安装应用 / 添加到主屏幕。
方式三：自己部署一份（Fork 或新仓库）
当前仓库：`cusimao/What-do-you-want-to-eat-today`
把本目录文件传到你的仓库（网页端拖拽上传，或下面命令行）。
仓库 Settings → Pages：`Source` 选 `Deploy from a branch`，分支选 `main`、目录选 `/ (root)`，保存。
等一两分钟，访问 `https://<你的用户名>.github.io/<仓库名>/`。
```bash
git clone https://github.com/cusimao/What-do-you-want-to-eat-today.git
cd What-do-you-want-to-eat-today
# 修改后提交推送
git add .
git commit -m "update"
git push
```
---
文件说明
文件	必需	说明
`index.html`	✅	应用本体。HTML + CSS + JS + 415 种食物数据全部内嵌，单独一个文件就能离线运行。
`README.md`	—	本说明文档。
`LICENSE`	—	MIT 许可。
`manifest.webmanifest`	可选	PWA 清单，让「添加到主屏幕」后以独立窗口运行。只在 http(s) 下使用。
`sw.js`	可选	Service Worker，让托管版本首次访问后可离线打开。只在 http(s) 下注册。
`icon-180.png` `icon-192.png` `icon-512.png`	可选	主屏幕图标。
`.nojekyll`	可选	让 GitHub Pages 跳过 Jekyll 处理（纯静态站点更稳）。
`.gitignore`	—	忽略系统与编辑器垃圾文件。
> `index.html` 通过 `location.protocol` 自动判断环境：以 `file://` 打开时，图标和 manifest 用内嵌数据（不请求任何外部文件）；通过 http(s) 访问时才使用仓库里的 `manifest.webmanifest`、`icon-*.png` 和 `sw.js`。所以**删掉可选文件也不会影响单文件离线使用**。
---
数据存储
所有数据都保存在浏览器的 `localStorage`，不会上传到任何服务器：
Key	内容
`ttc_profile_v1`	身体数据（性别、年龄、身高、体重、活动系数）
`ttc_custom_foods_v1`	你手动添加的自定义食物
`ttc_log_YYYY-MM-DD`	某一天的记录：`{ items: [...], tdee: 当天生效的 TDEE 快照 }`
几点说明：
每天的 TDEE 会存一份快照，所以以后修改体重不会篡改历史缺口；今天始终跟随最新的身体数据。
清空数据：浏览器设置里清除该站点的数据即可；也可以用 `localStorage.clear()`（会一并清掉身体数据）。
用 `file://` 直接打开时，个别浏览器会禁用 `localStorage`，此时页面顶部会显示黄色提示条 —— 换成 http(s) 打开或安装到主屏幕即可正常保存。
---
关于热量数据
食物热量为估算值，参考《中国食物成分表》整理，为了易用性四舍五入到整数，可能与具体品牌、做法有出入（比如同样是「红烧肉」，各家放糖放油差别很大）。
理论体重变化基于 `7700 kcal ≈ 1kg 脂肪` 的经典换算，是线性近似。真实的体重变化还受水分、糖原、肠道内容物、肌肉量变化与代谢适应影响，短期内体重波动很可能大于这里的估算值。
本项目只做记录与算术，不构成任何医疗或营养建议。有特殊健康状况请咨询医生或注册营养师。
---
技术说明
纯原生 HTML/CSS/JavaScript，无框架、无构建步骤、无依赖。
兼容性：任何支持 `localStorage` 与 `CSS Grid` 的现代移动浏览器（iOS Safari 12+、Chrome for Android 80+ 均可）。
代码组织（都在 `index.html` 的 `<script>` 里）：
`FOOD_GROUPS` / `FOOD_DB` —— 食物数据库
`store` —— localStorage 封装（不可用时降级到内存）
日期与每日数据读写 —— `dayStr` / `loadDay` / `saveDay` / `stampTdee`
搜索 —— `matchFoods`（子串优先 + 逐字顺序模糊匹配）
渲染 —— `renderProfile` / `renderResults` / `renderDayBar` / `renderRecords` / `renderSummary` / `renderStats` / `renderChart`
趋势统计 —— `rangeRows` / `renderStats`（近 7 / 30 天）
许可
MIT © 2026
