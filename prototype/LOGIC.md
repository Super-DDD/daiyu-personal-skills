# Logic prototype

用一个自包含的 HTML 文件做可分享的 demo，让任何人都能按按钮驱动 state model。业务逻辑、状态转换或数据形状看上去合理，却要经过具体场景才能知道哪里不对，这时适用。设计师、PM、领域专家都能直接打开，所以要使用他们的 domain language。

## 适用问题

- 「先发生 X 再发生 Y，state machine 能处理这个边缘情况吗？」
- 「data model 能表达这种情况吗？」
- 「想先试出 API 应该是什么样，再写实现。」
- 用户希望按按钮观察状态变化的其他场合。

若要探索视觉外观，读 [UI.md](UI.md)。

## 步骤

1. **写明问题。** 在 demo 顶部用可见的一段话说明要探索哪个 state model、回答哪个问题。这样当场参与或之后回来的人都能核对。
2. **抽出可移植的逻辑。** 把回答问题的逻辑写成同一个 `<script>` 中的小型纯 module，方便以后迁入真实代码。页面可以丢弃。离散事件驱动单一状态时用 pure reducer `(state, action) => state`；要判断当前合法操作时用显式状态和转换的 state machine；对普通数据类型做变换时用一组纯函数；逻辑自己持有长期状态时，可用一组方法明确的 class 或 module。按问题选择形式。页面向逻辑发起调用，逻辑不访问 DOM、`document` 或按钮 handler。
3. **编写可分享的 HTML。** 所有 HTML、CSS、JS 都内联。双击即可打开，也能发邮件传给别人。使用业务语言命名按钮和状态，并说明发生了什么。页面自上而下安排：
   - 标题及一行说明，呼应第 1 步的问题。
   - 当前状态面板。用带标签的字段展示全部相关状态，每次点击后重绘，需要时指出刚才改变的字段。
   - 自由操作区。每种 action 一个始终可用的按钮，允许以任意顺序操作，每次 dispatch 后重绘。
   - 引导演练。每个 tab 一个 scenario，简短介绍场景和观察点，下面按顺序放真正可操作的按钮。每按一次执行该 action 并进入下一步。从已知初始状态启动每次演练，使过程可重复。

   场景覆盖 happy path、难处理的边缘情况和应被拒绝的操作。排版清楚，留足间距，用一种强调色，省去分散注意力的动画和噱头。
4. **交给用户。** 把文件交给用户或替其打开，让对方按自己的节奏尝试。留意「等等，这应该做不到」或「我以为 X 会不一样」这类反馈，它们暴露设计想法里的问题。用户需要新的 action 或场景时再补。
5. **保留结论和 prototype。** 按 [prototype](SKILL.md) 的方法记录结论。已验证的 reducer、state machine 或函数可以独立迁入真实 module。HTML 外壳随临时 branch 保存为一手资料，之后还能双击重跑。

## Anti-patterns

不要给 prototype 加测试或为将来可能需要的 X 作泛化。除非验证的就是持久化，否则使用内存状态。把 DOM 和按钮处理留在薄页面里，确保逻辑能独立提取。保持单文件，无需 framework、bundler 或 server。HTML 外壳针对手动探索，生产代码中保留值得使用的逻辑 module。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/prototype/LOGIC.md)
