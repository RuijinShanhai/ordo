# Ordo

> 看清流程，理解全局。

Ordo 是流程阅读与路径探索产品，与文档查看产品 Folio、前端组件开发文档产品 Sutras 并列。名称来自拉丁语，含次序、秩序之意。

目标是帮助读者从全局流程定位节点、理解关系、探索路径，并关联业务说明与版本。它不是流程执行引擎，也不在首期承诺在线编辑、实时协作或业务自动执行。

## 当前交付

本仓目前是 **Markdown 产品方案归档**，不是可运行产品。初始化范围仅为本 README 和[架构与上游策略](docs/architecture-and-upstream-strategy.md)，没有应用代码、构建依赖或在线站点。

|事项|状态|
|---|---|
|独立产品仓方案|已归档；仓库提交/发布证据以实际 Git 引用为准|
|Archify 引擎 fork|目标为 `RuijinShanhai/archify`；本次创建调用被 GitHub API 未认证阻断，尚未确认创建|
|固定 SHA 引擎依赖|待引擎仓核验后接入；当前没有 submodule、gitlink 或锁定 SHA|
|流程模型、适配器及阅读界面|设计建议，未实施|
|PR、独立验收、在线部署|未完成；提交推送不等于集成或部署|

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

现在可直接阅读 Markdown；**没有安装、启动或部署命令**。模型示例、适配契约、升级检查点及验证回退见[完整方案](docs/architecture-and-upstream-strategy.md)。

## 上游与许可

- 上游：[tt-a1i/archify](https://github.com/tt-a1i/archify)。
- 指定引擎 fork 目标：[RuijinShanhai/archify](https://github.com/RuijinShanhai/archify)（目标链接不证明仓库已创建）。
- 方案继承此前对上游动态 main 的 README、Schema 说明及 Apache-2.0 LICENSE 的阅读记录；本轮没有重新拉取源码、固定上游版本或运行引擎。
- 实际接入/分发前重新核对固定 SHA 下的 LICENSE、NOTICE、第三方声明及修改标识。上游许可不自动决定 Ordo 自身许可；本仓尚未声明产品开源许可证。

首次空仓根提交采用明确授权的 bootstrap 例外；后续通过独立任务分支、PR、他人评审与独立测试集成，不将根提交视为完整 PR 验收。
