# Deepening

根据依赖情况，把一组 shallow module 安全地深化。术语定义见 [codebase-design](SKILL.md)。

## 按依赖选择测试方式

1. **In-process**。纯计算、内存状态、无 I/O。可以直接合并 module，从新 interface 测试，无须 adapter。
2. **Local-substitutable**。依赖有本地替身，例如用 PGLite 代替 Postgres、用内存文件系统。替身存在就可深化，在 test suite 中运行替身来测试。Seam 位于内部，无须在外部 interface 上加 port。
3. **Remote but owned（Ports & Adapters）**。通过网络访问自己的服务，例如 microservice 或内部 API。在 seam 定义 port（interface），把逻辑集中在 deep module，transport 作为 adapter 注入。测试用 in-memory adapter，生产用 HTTP、gRPC 或 queue adapter。提案可以写成「在 seam 上定义 port。生产用 HTTP adapter，测试用 in-memory adapter。即使跨网络部署，逻辑也集中在一个 deep module。」
4. **True external（Mock）**。Stripe、Twilio 等无法控制的第三方服务。将外部依赖作为 port 注入，由测试提供 mock adapter。

## 如何判断 seam

至少有两个合理的 adapter（通常分别用于生产、测试）再引入 port。只有一个 adapter 的 seam 只是多了一层间接调用。供 module 自身测试使用的 internal seam 可以保持私有。External seam 位于该 module 的 interface。

## 测试策略

新 interface 的测试已经覆盖相应行为时，删除旧 shallow module 上多余的 unit test。在 deep module 的 interface 上编写新测试，通过它断言可观察的结果。测试应能经受内部重构。实现一变就要改测试，说明它越过了 interface。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/codebase-design/DEEPENING.md)
