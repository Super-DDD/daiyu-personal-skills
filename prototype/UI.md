# UI prototype

在一条 route 上生成几种结构差异显著的 UI variant，通过底部悬浮栏切换。用户在浏览器中比较，选一种或取几种设计的不同部分，再丢弃其余部分。问题涉及逻辑或状态时读 [LOGIC.md](LOGIC.md)。

## 适用问题

例如「这个页面应该长什么样」「确定 dashboard 前想看几个方案」「试试设置页的另一种布局」。当用户只能在脑中比较几个模糊的 mockup 时，就适合做这个 prototype。

## 优先依附现有页面

在应用真实的 header、sidebar、数据和信息密度中，UI 更容易判断。独立的空页面容易掩盖问题。

**A. 调整已有页面（首选）。** 在同一条 route 上用 `?variant=` 控制渲染。保留已有的数据获取、参数和认证，只切换渲染的部分。Dashboard 的新区域、设置页的新卡片或既有流程中的新步骤，都可以挂进宿主页面。

**B. 新页面（最后选项）。** 仅适用于全新的顶层界面或无法合理嵌入的流程。先确认没有合适的宿主，再依项目约定建临时 route，路径或文件名带 `prototype`，使用同样的 `?variant=`。两种形式共用底部切换栏。

## 步骤

1. **说明问题和数量。** 默认做 3 个 variant，最多 5 个。用一行写明计划，放在 prototype 所在位置或文件顶部注释。例：「在现有 `/settings` route 上为设置页做三种 variant，通过 `?variant=` 切换。」用户稍后也可以阅读。
2. **设计不同的结构。** 每种 variant 都遵循页面用途、可访问的数据和项目已有的组件库与样式系统（TailwindCSS、shadcn、MUI 或普通 CSS 等）。为导出的组件起清楚的名字，如 `VariantA`、`VariantB`、`VariantC`。改变布局、信息层级或主要操作方式。若两个草案都只是略改 card grid，就明确要求其中一个重新设计并避开 card grid。
3. **连接 variant。** 在 route 上使用一个切换组件。

```tsx
// pseudo-code, adapt to the project's framework
const variant = searchParams.get('variant') ?? 'A';
return (
  <>
    {variant === 'A' && <VariantA {...data} />}
    {variant === 'B' && <VariantB {...data} />}
    {variant === 'C' && <VariantC {...data} />}
    <PrototypeSwitcher variants={['A','B','C']} current={variant} />
  </>
);
```

   形式 A 保留切换组件上方的数据获取，只切换渲染子树。形式 B 在 `/prototype/<name>` 下的临时 route 挂载同一切换组件。

4. **底部切换栏。** 在屏幕底部居中固定显示左箭头、当前 variant 和右箭头。左右循环切换，有名称时连同 key 显示，如 `B (Sidebar layout)`。点击后用框架 router 更新 URL query parameter（Next 的 `router.replace`、React Router 的 `navigate` 等），让链接可分享、刷新后仍是同一 variant。支持 ← / → 键；当焦点位于 `<input>`、`<textarea>` 或 `[contenteditable]` 时不拦截。用高对比度胶囊、轻阴影等与待评价的界面区分。在生产构建中用 `process.env.NODE_ENV !== 'production'` 或等效检查隐藏切换栏。将其放在项目共享 UI 的位置，供两种形式复用。
5. **交给用户。** 提供 URL 和 `?variant=` 的可用值。用户可能会说「想要 B 的 header 加 C 的 sidebar」，这往往就是他们要的设计。
6. **记录结论并清理。** 记下获选的 variant 和理由，按 [prototype](SKILL.md) 保存一手资料。形式 A 把获选设计融入现有页面，main 中移除其他 variant 和切换栏。形式 B 将获选设计实现为正式 route，main 中移除临时 route 和切换栏。完整 variant 集合保存在临时 branch，留在 main 会逐渐过时并误导读者。

## Anti-patterns

只改颜色或文案无法比较结构。共享 header 可以，共享整个 layout 则会限制独立设计。可读取真实数据，需要写入时指向 stub，聚焦外观问题。Prototype 代码省去了测试和大部分错误处理，融入生产代码时要按正式要求重新实现。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/prototype/UI.md)
