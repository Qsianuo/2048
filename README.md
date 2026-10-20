# 2048

一个零依赖、无构建、打开即玩的 2048 小游戏，使用原生 HTML / CSS / JavaScript 实现。

A dependency-free 2048 game built with vanilla HTML, CSS, and JavaScript.

> 在线试玩：<https://qsianuo.github.io/2048/>（启用 GitHub Pages 后生效）

## 特性

- 标准 4×4 棋盘，目标 2048，四方向移动与合并
- 方向键、WASD、触摸滑动三种操作方式
- 计分与跨会话最高分（`localStorage`）
- 最多 20 步撤销
- 胜利后继续挑战；无路可走时结束并重开
- 响应式布局，适配桌面与手机；尊重系统“减少动态效果”
- 零运行时依赖、零构建步骤、零第三方请求、无广告、无账号
- 核心规则封装为 UMD 模块，可直接在 Node.js 中 `require`

## 运行

直接双击 `index.html` 即可游玩。

也可以启动任意静态服务器：

```bash
# 方式一：Python 3
python -m http.server 8080

# 方式二：Node.js（任选其一）
npx serve .
```

然后访问 <http://localhost:8080/>。

## 操作

| 操作 | 按键 / 手势 |
|------|-------------|
| 移动 | `↑` `↓` `←` `→` 或 `W` `A` `S` `D` |
| 触摸移动 | 在棋盘上向目标方向滑动至少 24px |
| 撤销 | 点击“撤销”按钮 |
| 新游戏 | 点击“新游戏”按钮 |
| 弹层确认 | `Enter` 或点击主按钮 |

## 规则

- 每次有效移动后，在随机空格生成一个方块：90% 为 2，10% 为 4。
- 相邻且数值相同的方块合并为两倍；每个方块每步最多合并一次。
- 合并得分等于合并后的数值；一次移动中多组合并累加。
- 合成 2048 时弹出胜利提示，可选择继续游玩。
- 棋盘填满且不存在相邻同值方块时游戏结束。

## 项目结构

```text
.
├─ index.html          # 页面结构
├─ style.css           # 布局、配色与动效
├─ logic.js            # 纯游戏逻辑（UMD，可在 Node 中直接引用）
├─ game.js             # 状态机、渲染、输入与存储
├─ docs/
│  └─ PRDv1.0.md       # 产品需求文档 v1.0
├─ .gitattributes      # 统一使用 LF 换行
└─ README.md
```

`logic.js` 与 DOM 完全解耦：

```js
const G = require('./logic.js');

const board = G.createBoard();
board[0] = [2, 2, 4, 4];

const result = G.move(board, 'left');
console.log(result.board[0]); // [4, 8, 0, 0]
console.log(result.score);    // 12
```

## 隐私

游戏不包含广告、第三方统计或外部网络请求。最高分仅保存在当前浏览器的 `localStorage` 中（键名 `game2048.best`），不会上传到任何服务器。页面不使用 Cookie，也没有账号体系。

## 已知限制

- 当前棋局不保存，刷新或关闭页面后需要重新开始（最高分会保留）。
- 未达到 WCAG 2.2 AA：部分文字与数字方块对比度不足；弹层缺少焦点陷阱与 Escape 关闭；棋盘变化没有读屏播报。
- 点击“新游戏”会立即重置，没有二次确认。
- 未提供自动化测试与 CI；核心逻辑经过开发期一次性验证，尚不可重复回归。
- 尚未添加 LICENSE。

完整的需求、边界条件、测试用例与已知缺陷见 [PRD v1.0](docs/PRDv1.0.md)。

## 许可证

本仓库尚未添加 LICENSE。在许可证确定前，保留所有权利；如需复用代码，请先联系维护者。

## 维护者

[Qsianuo](https://github.com/Qsianuo)
