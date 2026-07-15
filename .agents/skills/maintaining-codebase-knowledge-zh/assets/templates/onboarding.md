<!-- codebase-knowledge:managed -->
# 入门和项目 baseline

最后验证：重要时记录 revision 和日期

## 稳定前置条件

记录仓库要求的版本、服务和配置，并附证据。不要将某个 agent 本地缺少的工具转化为项目要求。

## 命令

| Purpose | Command and working directory | Expected signal | Evidence |
|---|---|---|---|
| Setup、build、test、lint、type-check、run、benchmark 或 verification | 已验证命令 | 稳定的成功或失败信号 | Manifest、CI definition、文档或可复现运行 |

## 可复现项目 baseline

只记录在指定 revision 和相关环境中复现的失败或约束。偶发的本地环境失败留在运行结果。
