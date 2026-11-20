# 固定版本引擎接入与真实验证

2026-11-20；Task-ID `ORDO-INIT-20261118-001`；Run-ID `ORDO-INIT-20261118-001-ENGINE-20261120-R6`。
精卫/00000001/frontend-component-engineer。平台session/task ID未提供；自检不代独立验收。

## 版本与实际gitlink

- fork <https://github.com/RuijinShanhai/archify>，parent `tt-a1i/archify`，维护交接已API核验。
- tag `v3.2.0`，tag对象 `e62275b6373fa63436e195635835c313ce028b21`，commit **`208acba8d412335185b2056fedf54b3405df43fb`**。选用本次具名恢复的最高非预发布后缀版本；不以main替代，不声称签名可信或全功能稳定性验收。
- 入口1.5.0，两仓check exit0；产品起始HEAD `ca4cc4a3727b5ceb643949ef0eab720440f0ad30`。组织resolve exit0/registry1.4.1，外仓例外不是共享组织准入通过。
- 维护已恢复固定tag可达1732提交，非shallow，commit/tree历史完整；历史blob按需，不意味着提交历史丢失。引擎worktree sparse仅物化archify/及根许可等范围，不声称全部分支和blob下载完毕。
- 受控 `submodule --repo ordo --sha 208acba8d412335185b2056fedf54b3405df43fb` **exit0/submodule-staged**。真实index为`160000 208acba8d412335185b2056fedf54b3405df43fb vendor/archify`，`.gitmodules` URL精确为`https://github.com/RuijinShanhai/archify.git`；子模块detached HEAD相同，1732提交、非shallow。

### 本地物化缺口（未隐藏）

1.5.0 submodule调用本地clone/no-checkout、remote set-url、checkout，返回成功且CLI可运行。但后续真实 `git -C vendor/archify status --short` 显示大量非archify路径为` D`，父仓显示`Am vendor/archify`。例如CONTRIBUTING.md、REVIEWING.md、test/、viewer/、website/等未在工作树物化，`ls-files -v CONTRIBUTING.md`为H而非skip-worktree；子模块index无暂存改动。它不是干净完整checkout，也不能直接把这些删除叫作正确sparse状态。

没有手工删除、修改子模块源码或隐藏dirty标志；未原生checkout/reset/config补救。固定gitlink仍指原始commit，不包含这些未暂存删除。需有权维护者修复受控子模块物化/索引一致性并重验干净状态。新环境通过远端取得gitlink也尚未独立实测；本轮不宣称完整可复现或完全交付验收。

## 固定版许可实读

根LICENSE及archify/LICENSE为MIT，版权2026 tt-a1i及2025 Cocoon AI，分发需保留版权及许可。旧动态main Apache-2.0记录不适用于此SHA。

已读根THIRD_PARTY_NOTICES.md：Simple Icons16.28.0集合CC0不代表每个标志都CC0，个别含CC-BY、CC-BY-SA、CC-BY-NC-SA及商标限制；OpenAI标志另循品牌指南。JetBrains Mono嵌入字体为OFL1.1，完整文本在`archify/assets/JetBrainsMono-OFL.txt`（已读），HTML携带字体CSS许可。Pele1.3.0为MIT，许可在`archify/explainer/vendor/pele/`。根及archify/没有独立命名NOTICE文件，实际第三方声明是THIRD_PARTY_NOTICES.md。本次选择不指定品牌标志的workflow例子；不作一般商用权利清理或法律合规保证。

## 实际命令与结果

工具：Git2.39.5、Python3.11.2、Node v24.22.2；package version3.2.0/type module/Node>=18。无需npm依赖安装，无live Skill安装。以下`CLI`表示`node vendor/archify/archify/bin/archify.mjs`，从产品任务worktree运行：

```sh
node vendor/archify/archify/bin/archify.mjs --help
node vendor/archify/archify/bin/archify.mjs validate workflow vendor/archify/archify/examples/agent-tool-call.workflow.json --json
node vendor/archify/archify/bin/archify.mjs render workflow vendor/archify/archify/examples/agent-tool-call.workflow.json /workspace/deliverables/ordo/agent-tool-call-vendor-r6.html
node vendor/archify/archify/bin/archify.mjs check /workspace/deliverables/ordo/agent-tool-call-vendor-r6.html --json
```

帮助最初从引擎worktree同版CLI读取exit0。引擎worktree及vendor路径各运行validate/render/check均成功exit0；validate和check各9项静态检查通过，composition errors0/warnings0。输入4939 bytes，SHA256 `d67965d50872c97e4ff3476271e645bf19fab6b3ab44e111bdb44b063e3aa36f`。

两路径生成HTML相同：830859 bytes，SHA256 **`38220f39f8110f9f0b1fdbd96fd750bd33a3750de45dc2913827e3fc30224c85`**。产物保存`deliverables/ordo/agent-tool-call-r6.html`及`agent-tool-call-vendor-r6.html`，不把830KB测试产物提交产品仓。

负例：将固定版package.json作为workflow输入，validate输出ok:false/schema required及additionalProperties诊断，符合拒绝非模型输入预期，不是产品Schema实现。精确子退出码/原始JSON由任务验证日志记录。

check的provenance为`unknown`：这里只用render/check，不将它冒报有delivery证明的finalize成功。HTML已生成但未在浏览器打开；静态几何检查不等视觉一致、键盘/读屏或可访问性验收。

## 交接与剩余项

本次实现只有固定引擎gitlink和CLI最小链路，无引擎补丁、Ordo模型/适配器或阅读界面，不制造引擎补丁PR。PR1由贡献head提交推送更新，不重复create；只读核实际远端head。没有默认分支变更、合并、生产部署、CI通过声明或独立验收。

维护需处理上述本地子模块dirty物化缺口；独立方需核同SHA的新环境获取、许可保留、浏览器/安全/交互、必要回归与CI。模型/API及loading/empty/error/success/focus等状态为后续实现，未报覆盖；回退演练未执行。
