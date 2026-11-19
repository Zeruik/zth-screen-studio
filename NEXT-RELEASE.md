# 3.0.1 维护记录与后续计划

3.0.1 已发布：[Release](https://github.com/Zeruik/zth-screen-studio/releases/tag/v3.0.1)。发布标签提交：`7792117273078e89429cc25633c67406a8812d6e`。本文件是发布后的文档更新；精确构建输入请使用发布标签。

## 本次改进

导出工程拒绝 Windows 设备保留名、非法字符和尾随点/空格。 三个平台的桌面包覆盖 Windows x64、Linux x64/arm64、macOS x64/arm64。Linux 负载权限显式设置，并保留已正确配置的 root-owned sandbox helper。

## 已完成验证

同一标签提交的基础回归、Linux/macOS 原生启动/重开和交付检查、Windows 实际生产程序检查与包摘要检查通过。发布器逐项核对远端大小和摘要后公开 Release。九个平台包与 SHA256SUMS.txt 共十个附件，其公开摘要和标签目标已另行复核；历史版本保留。

这些检查不表示真实开发板、显示器、机器人或用户既有机器的安装升级验收。macOS 为 ad-hoc 签名，Windows Setup 未签名；具体边界见 Release 正文。

## 后续优先项

首次亮屏诊断、资源预算显示、可复现 Arduino 编译矩阵和模板迁移。
