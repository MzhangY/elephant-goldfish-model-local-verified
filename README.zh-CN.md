# Elephant-Goldfish Model（EGM）：Windows 本地验证

这是 google/elephant-goldfish-model 的学习验证 Fork，保留 Google 和上游贡献者署名及 Apache-2.0 LICENSE，不是原创模型。固定上游提交 `70406c4d22cc7ea8299637c2c5b44625c48b3f08`。

## 真实结果（2026-11-20，Windows / Python 3.13.5）

- 首轮：368 通过、18 失败、3 跳过。
- 改为测试进程优先 Git Bash，避免误选 WSL bash stub 后，完整复测 `tests/unit`：**370 通过、16 失败、3 跳过**。
- 剩余 16 项均为 **WinError 1314：没有创建符号链接的权限**。
- 没有改 skip，没有开启管理员/开发者模式，也没有用复制伪装符号链接。
- **不是全绿验证，不表示所有 Agent 宿主安装成功。** 不建议直接接入当前日常环境。

验证时 Git fetch TLS 失败，改用官方固定提交归档，并校验全部 291 个 Git blob 哈希。上传以真实上游提交为父提交，保留上游历史关系。

## 已验证 / 未运行

- 成功构建 OpenAI 目标插件 ZIP，归档完整、LICENSE 存在；此 Fork 不提交二进制包。
- 未执行 marketplace 安装，未修改当前 Codex 配置，未启用子 Agent 评审。
- `egm-brainstorm`、`egm-draft`、`egm-review` 的真实宿主验收：**not run**。
- 未改上游运行时代码，未创建上游 PR。

## 独立环境复现

```powershell
python -m venv .venv-local-validation
.\.venv-local-validation\Scripts\python.exe -m pip install -r validation/requirements-tests.txt
.\validation\verify.ps1 -PythonPath .\.venv-local-validation\Scripts\python.exe
```

需要 Git Bash，其他安装位置设置 `$env:VALIDATION_GIT_BASH_DIR`。本次 Windows 权限下 16 项失败仍是已知限制，脚本不会隐藏失败，任一失败返回非零。应换到本来支持 symlink 的隔离环境复测，而不是降低本机安全设置。

## 证据

`validation/results.json` 和 `validation/evidence/` 包含原始/复测 JUnit、日志、构建与来源证明。临时 home/config、移除 API 环境变量及 Python socket 限制不是 OS 沙箱或安全审计。
