# 引擎现场核验与接入状态

日期：2026-11-20；Task-ID `ORDO-INIT-20261118-001`；Run-ID `ORDO-INIT-20261118-001-ENGINE-20261120-R5`。
执行者：精卫 / 00000001 / frontend-component-engineer。平台session/task ID未提供。本文为自检，不是独立验收。

## 已确认事实

- 产品基线 `3aacc2af6611d09a9acb71088cd33decb0ad1057`，本轮开始HEAD `4f062b19ebd75c0d16e55e295c0b7bc8e4ee4ee3`，任务worktree干净。
- 贡献分支 `agent/00000001/ORDO-INIT-20261118-001-engine-v1`；PR <https://github.com/RuijinShanhai/ordo/pull/1>，不推默认分支，不合并部署。
- 维护交接 `FORK-RESULT.md` 已记录真实fork <https://github.com/RuijinShanhai/archify>、parent `tt-a1i/archify`，API创建后和幂等核验成功。本轮不重复创建fork。
- 入口 `ordo_task.py --help` 确认1.4.0。`check --repo ordo` exit0，真实任务隔离通过；组织resolve exit0，registry1.4.1，组织check exit2 `foreign repository worktree`。外仓例外不冒称共享门禁通过。

## 本轮真实执行记录

以下入口路径均为 `/workspace/deploy/tools/opencode/ordo_task.py`，使用Python3.11.2；Git2.39.5，Node v24.22.2。命令原始输出保存在执行会话；非秘密摘要同步至任务交付目录。

|命令|结果|
|---|---|
|`python3 …/ordo_task.py clone --repo archify` 首次|父bash工具120000ms超时终止，未得到入口退出码，不报成功|
|同命令再次，父工具240000ms预算|入口exit2 rejected：内部Git clone在180秒超时|
|`python3 …/ordo_task.py probe --repo archify`|exit0 `git-ssh-readable`，非空refs，API authentication未检查|
|`python3 …/ordo_task.py worktree --repo archify`|入口exit2 rejected：内部 `fetch --no-tags --no-recurse-submodules … refs/heads/main` 在180秒超时|
|`git -C /workspace/archify rev-parse --verify HEAD`|exit128 `Needed a single revision`|
|`git -C /workspace/archify show-ref --tags`|exit1，无本地tag引用|
|产品 `git status --short`|exit0，无改动（文档修改前）|

clone留下 `/workspace/archify/.git`，没有源码检出，未覆盖、删除、清理或重新初始化该目录。worktree恢复尝试仍经原受控入口；失败后未用原生fetch/clone、其他协议、浅历史或修改入口超时来绕过。没有可用引擎worktree，因此没有执行submodule，也没有伪写`.gitmodules`或gitlink。

## 已取得的远端版本候选（不是本地锁定）

真实SSH probe返回：

|引用|40位SHA|
|---|---|
|`HEAD`、`refs/heads/main`|`44781293f38728baf04dbad33ce1a0cef8c39cca`|
|`refs/tags/v3.2.0`|`e62275b6373fa63436e195635835c313ce028b21`|
|`refs/tags/v3.2.0^{}`|`208acba8d412335185b2056fedf54b3405df43fb`|
|`refs/tags/v3.1.0`|`2e1dba8653b800400f41c84a058561e960c21963`|
|`refs/tags/v3.1.0^{}`|`1c2b42d660e4a7571813fb43839c79e3e9ca7720`|

`v3.2.0`为本次所见最高无预发布后缀的主版本tag候选；仅凭refs不能证明发布稳定性、签名可信、适用性、许可或本地对象完整。尚未选定集成版本，不以动态main替代稳定tag。

## 许可、CLI及验收边界

旧方案记有未锁定动态main的Apache-2.0阅读记录，**不能代替上述候选SHA的LICENSE/NOTICE/第三方声明核验**。本轮未读到固定版本源码、包清单、CLI帮助或许可证；不确认候选仍为Apache-2.0，不作法律合规保证。没有依赖安装、样例校验、HTML生成、引擎测试或浏览器渲染结果。

没有组件API实现；loading/empty/error/success/focus、视觉、键盘、读屏和容器安全均未测试。没有独立验收、CI结论、合并、上线、默认分支或远端保护变更。

## 最小续办条件

1. 由有权维护者核查受控clone/fetch 180秒超时，保留现有半成品仓，不要求精卫改控制入口或清理目录；提供能在原入口完成完整历史传输的方案和实际结果。
2. 源码到位后经受控worktree/check，取得真实tag对象与完整commit，核LICENSE/NOTICE/第三方声明、版本发布说明与CLI帮助。
3. 按该版本已有样例实际运行校验和HTML生成，记录各子退出码、工具链、日志与产物摘要；浏览器交互另测。
4. 仅在具名引擎worktree存在已审commit后，用受控 `submodule --repo ordo --sha <真实40位commit>` 固定 `vendor/archify`；审查精确`.gitmodules`与160000 gitlink。
5. 产品受控review/commit/push更新PR1，独立评审和测试由未参与实施者承担。新环境取得同一提交及回退演练仍需单独实证。
