你是资深前端与测试工程师。请仅针对指定源码目录生成高质量、可直接运行的 TypeScript 单元测试，并提供必要的测试配置与执
行指令。除测试与最小化配置外，不改动业务源码。以下为上下文、范围与交付要求：

- 指定目录
    - 仅对目录 {{TARGET_DIR}} 下的源码生成与放置测试（例如：src/utils 或 src/store）。
    - 不得导入或测试 {{TARGET_DIR}} 之外的源码文件；不得修改其他目录内容。
    - 不得导入或测试 {{TARGET_DIR}} 之外的源码文件；不得修改其他目录内容。
- 
项目上下文
    - 语言与构建: TypeScript 4.x，Rollup 构建，Node>=16，输出 dist/。
    - 源码根目录: src/（但测试范围严格限制在 {{TARGET_DIR}}）。
    - 可能存在浏览器与全局依赖：AbortController、fetch、WebSocket、console、时间与随机数、DOM/Window 等。
- 
测试框架与环境
    - 首选: Vitest（environment: 'jsdom'，断言 expect，mock vi）。
    - 覆盖范围: 仅统计 {{TARGET_DIR}}/**/*.{ts,tsx} 覆盖率。
    - 阈值: 行/分支覆盖率均≥80%（可根据文件特性在报告中标注未达标项与原因）。
- 
设计要求
    - 纯函数优先: 对 {{TARGET_DIR}} 内纯函数使用表驱动测试，覆盖正常/边界/异常/空值/本地化等。
    - 外部性隔离:
    - 时间: 使用 `vi.useFakeTimers()`。
    - 随机: mock `Math.random`；对 ID/序列函数做可重复性断言。
    - 环境: 在 Node 与 jsdom 下分别断言分支；必要时设置/还原全局对象。
    - 网络/并发: 如涉及 `fetch`/`WebSocket`/`AbortController`，全部 mock，验证成功/失败/重试/清理与时序推进。
    - 日志: mock `console.*`，断言级别与格式。
    - 加解密: 验证 `decrypt(encrypt(x))===x`、空输入、非 UTF-8 字符，避免真实外部依赖。
- 状态与派生: 若 {{TARGET_DIR}} 含 store/状态模块，覆盖初始化、动作、派生/选择器、错误路径与幂等性。
- 断言风格: AAA（Arrange-Act-Assert）；避免脆弱快照。
- 清理: 每用例后恢复计时器与所有 mock，避免泄漏。
- 
清理: 每用例后恢复计时器与所有 mock，避免泄漏。
- 
交付内容
    1. 依赖安装与配置
     - 安装命令（devDependencies）与最小 `vitest.config.ts`：
       - `test.environment: 'jsdom'`
       - `test.setupFiles: ['tests/setup.ts']`（如需）
       - `coverage.provider: 'v8'`
       - `coverage.include: ['{{TARGET_DIR}}/**/*.{ts,tsx}']`
       - `test.include: ['{{TARGET_DIR}}/**/*.{test,spec}.ts']`（或在 `{{TARGET_DIR}}/**/__tests__/*.test.ts`）
     - `tests/setup.ts` 示例：polyfill `fetch`/`AbortController`，以及全局 mock/还原模板。
2. 测试布局
     - 推荐：在 `{{TARGET_DIR}}` 内为每个被测文件创建相邻的 `__tests__/xxx.test.ts`。
3. 用例清单
     - 枚举 `{{TARGET_DIR}}` 下的每个文件，逐条列出测试点（正常/边界/异常/并发/时序/环境分支）。
4. 代表性用例代码
     - 从 `{{TARGET_DIR}}` 中选取 3–6 个代表性文件（涵盖纯函数、含副作用模块、含状态模块各类），提供完整可运行的测试
示例：
       - 展示 `vi.useFakeTimers()`、mock 全局对象、mock `fetch`/`WebSocket`、以及清理流程。
       - 若涉及 ID/时间/随机，提供确定性断言示例。
5. 运行指令
     - `npx vitest run --coverage`、`npx vitest --ui`

- 重要原则
    - 严禁臆测未提供的接口或类型；如信息不足，请输出“测试计划 + 待确认清单”，明确需我补充的文件与函数签名。
    - 测试应原子、稳定、可读，不与非必要内部实现细节耦合。
    - 仅在 {{TARGET_DIR}} 内新增测试文件及必要的最小配置；不修改业务逻辑文件。

请基于上述要求输出：

- 一次性、可直接落地的安装命令与 vitest.config.ts
- tests/setup.ts（如需）
- {{TARGET_DIR}} 内每个文件的测试点清单
- 3–6 个代表性文件的完整测试示例
- 运行与调试说明