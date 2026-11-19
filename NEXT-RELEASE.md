# 下一轮维护计划 / Next maintenance iteration

目标：3.0.1（候选规划，尚未发布；版本字段和旧标签保持当前值）。

## 已实现 / Implemented

工程 ZIP 路径拒绝 Windows 设备名、非法字符、尾随点/空格，避免生成目标系统不能正常解压的文件。

另修复 Linux 打包权限：生成的 TAR/DEB 负载和控制目录使用明确的公开读取/执行权限，不继承开发机器的私有 umask；新增 umask 077 下实际构建 DEB 的回归测试。

## 当前云实例验证 / Current cloud validation

15 core groups; 288 board/display/rotation/template combinations; DOM interaction; platform and window-preference contracts passed.

Additional checks: node qa-package-permissions.cjs passed with a real synthetic DEB under umask 077. Actual Linux x64 TAR and DEB builds passed using checksum-pinned runtimes; SHA256SUMS and installed payload permissions were verified. These are validation builds with the current version number, not next-version release assets. No GUI/install lifecycle acceptance was performed.

New portable-regression CI workflows, where present, have not yet been observed running on GitHub. They use synthetic data and do not replace native acceptance.

## 建议优化 / Recommended follow-up

后续优先：首次亮屏诊断、导出资源预算、模板迁移。需另行完成 Arduino 编译矩阵与真实显示器验收。

## 发布条件 / Publication gates

先完成对应平台构建与升级/数据保留验收，再同步版本字段、刷新源码清单、为新版本生成附件与 SHA-256 校验。历史 Release、标签和附件不可覆盖。当前 api.github.com 在代理 CONNECT 阶段返回 403，已将该域名加入环境配置草案；草案保存不等于运行时已应用。恢复 API 后还需核验当前用户、仓库权限、既有 Release 和候选标签是否冲突。

本文件记录源码改进和实际执行范围，不表示新版本已经发布，也不将软件/模拟测试称为实板或手机验收。
