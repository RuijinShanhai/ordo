# Ordo

> 看清流程，理解全局。

Ordo 是流程阅读与路径探索产品，与文档查看产品 Folio、前端组件开发文档产品 Sutras 并列。名称来自拉丁语，含次序、秩序之意。

目标是帮助读者从全局流程定位节点、理解关系、探索路径，并关联业务说明与版本。它不是流程执行引擎，也不在首期承诺在线编辑、实时协作或业务自动执行。

## 当前交付

本仓目前是 **产品方案归档 + 固定版本引擎CLI接入**，不是完整可运行产品。已通过 `vendor/archify` 固定引擎并实际校验样例、生成HTML；没有Ordo阅读界面、适配器或在线站点。

|事项|状态|
|---|---|
|独立产品仓方案|已归档；仓库提交/发布证据以实际 Git 引用为准|
|Archify 引擎 fork|已真实创建，parent 为 `tt-a1i/archify`；传输已恢复，固定tag历史1732提交、非shallow|
|固定 SHA 引擎依赖|`vendor/archify` submodule固定v3.2.0 commit `208acba8d412335185b2056fedf54b3405df43fb`；CLI正例及HTML检查通过；本地子模块存在未物化文件呈删除的维护缺口|
|流程模型、适配器及阅读界面|设计建议，未实施|
|PR、独立验收、在线部署|本轮文档通过独立贡献分支申请PR；独立验收与部署未完成，提交推送不等于集成或部署|

## 架构方向

```text
Ordo 产品界面（目录、搜索、业务说明、文档关联、版本导航）
  → 版本化 Ordo 流程模型（稳定节点及关系 ID）
  → Archify 适配层（转换、校验、调用、错误映射）
  → 固定完整 SHA 的 Archify 引擎 fork
  → 可交互 HTML
```

产品语义不直接绑定上游 Schema。业务字段留在产品模型，通过稳定 ID 关联；引擎 fork 只承担必要的布局、渲染和底层交互补丁。首期建议先通过 CLI 生成 HTML，提供查看入口或隔离的 iframe 容器，不假定上游已有可嵌入组件 API。

## 实施顺序与使用入口

1. 初始化产品仓并归档方案（本次范围）。
2. 创建保留 `tt-a1i/archify` 完整上游历史的组织 fork，核许可证、源码、版本和测试。
3. 在 `vendor/archify` 通过 Git submodule 固定已审阅的完整提交 SHA，不自动跟踪 main。
4. 另行实现最小流程模型、适配器及只读阅读界面。
5. 完成回归、独立评审/验收后，再按单独授权部署。

现在可阅读Markdown或运行已有引擎CLI（Node >=18，本轮v24.22.2，无需安装依赖）：

```sh
node vendor/archify/archify/bin/archify.mjs validate workflow vendor/archify/archify/examples/agent-tool-call.workflow.json --json
node vendor/archify/archify/bin/archify.mjs render workflow vendor/archify/archify/examples/agent-tool-call.workflow.json workflow.html
node vendor/archify/archify/bin/archify.mjs check workflow.html --json
```

以上为引擎入口，不是Ordo应用启动命令。样例、许可、产物摘要及本地checkout缺口见[现场核验](docs/engine-verification.md)。新环境submodule获取尚未独立验证，不使用`--remote`追踪main。模型与适配建议见[完整方案](docs/architecture-and-upstream-strategy.md)。

## 上游与许可

- 上游：[tt-a1i/archify](https://github.com/tt-a1i/archify)。
- 指定引擎 fork：[RuijinShanhai/archify](https://github.com/RuijinShanhai/archify)；维护交接已核真实fork与parent，本轮已核SSH可读。
- 固定版根LICENSE及`archify/LICENSE`实读为 **MIT**，不是旧动态main阅读记录中的Apache-2.0；保留版权及许可文本。
- 已核`THIRD_PARTY_NOTICES.md`：品牌图形不被MIT自动覆盖；Simple Icons、各品牌独立条款/商标要求、JetBrains Mono OFL1.1及Pele MIT须分别保留。本轮例子不选择品牌图标，不表示任意商用品牌使用获许可。
- 上游许可不自动决定Ordo自身许可；本仓尚未声明产品开源许可证。引擎源码无补丁，浏览器、独立验收、部署未完成。

首次空仓根提交采用明确授权的 bootstrap 例外；后续通过独立任务分支、PR、他人评审与独立测试集成，不将根提交视为完整 PR 验收。
