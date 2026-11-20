# 引擎现场核验与接入状态

日期：2026-11-20。此记录是实际执行结果，不是稳定版本或接入完成声明。

## 当前事实

- 产品基线：`3aacc2af6611d09a9acb71088cd33decb0ad1057`。
- 贡献分支：`agent/00000001/ORDO-INIT-20261118-001-engine-v1`；基线分支为此前初始化分支，不把贡献直接推到远端默认。
- 引擎目标：<https://github.com/RuijinShanhai/archify>；上游：<https://github.com/tt-a1i/archify>。目标URL不证明创建成功。
- 受控入口版本1.3.0：`auth`退出0，返回`authentication: available, principal: RuijinShanhai`。
- `fork --repo archify`入口退出2，内部gh退出1。入口原样返回：`child failed: /workspace/deploy/tools/opencode/gh-2.83.1/bin/gh exit=1 (raw output intentionally withheld; may contain credential URLs)`。这里未得到GitHub原始stderr，不将原因猜作认证失败或账号类型错误。
- `probe --repo archify`入口退出2，内部Git退出128，原始错误同样被入口隐藏。读取失败不足以断言仓库不存在。
- 稳定tag、完整引擎SHA、固定版本LICENSE/NOTICE/第三方声明、CLI说明：**尚未现场取得，未验证**。没有`.gitmodules`或160000 gitlink，没有引擎源码改动，也没有生成HTML。

## 待恢复后的复现顺序

以下是本任务受控入口的操作计划；除上述auth/fork/probe及产品check外，不是已执行成功的记录。入口位于任务宿主，不是产品自带依赖。

```sh
python3 /workspace/deploy/tools/opencode/ordo_task.py auth
python3 /workspace/deploy/tools/opencode/ordo_task.py fork --repo archify
python3 /workspace/deploy/tools/opencode/ordo_task.py clone --repo archify
python3 /workspace/deploy/tools/opencode/ordo_task.py worktree --repo archify
python3 /workspace/deploy/tools/opencode/ordo_task.py check --repo archify
```

维护者需先给出脱敏子进程错误，并核对fork命令固定`--org RuijinShanhai`是否适合实际账号类型。个人账号与组织不可互换；执行者不改入口、不用原生命令绕过拒绝。目标若已存在则保留，先核parent，不覆盖或重建。

仓库到位后必须读取真实tags与完整提交对象，选择非预发布稳定tag，记录tag对象与解引用commit、版本选择依据；无稳定tag时明确记录，不把fork默认HEAD称为稳定版。核父仓与历史、固定SHA下LICENSE/NOTICE/第三方声明及CLI说明；复用该版本已有最小例子生成HTML，执行该版本提供的校验/测试并记录命令、版本、退出码、产物摘要。根据实际CLI帮助填写复现命令，而非预先虚构参数。

以上核验后才用受控`submodule --repo ordo --sha`传入真实40位SHA，审查`.gitmodules`和160000 gitlink。不得手写占位SHA，不自动追踪main，不把内部交付证据带进引擎fork。新环境获取同一提交的能力需另验证，不能仅凭本地缓存宣称可复现。

## 未完项与接受边界

本轮没有引擎最小样例、模型适配器或阅读界面；没有组件状态、交互、视觉、键盘、读屏、容器安全或独立验收结果。保持既有产品模型与架构建议，不为阻塞新增完整产品或替代引擎。

本PR仅修正方案当前状态及交接步骤，不代表固定SHA接入的验收。恢复后通过后续受控贡献提交真实版本与gitlink，再申请独立评审/测试。没有合并、上线、远端规则或默认分支变更。
