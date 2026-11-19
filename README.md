# HengDun Releases（衡盾 · 产物仓库）

> **本仓库只存放编译好的发布产物，公开只读。严禁提交任何源码。**

源码在私有仓库 [`CaiWanq/hengdun`](https://github.com/CaiWanq/hengdun)，
CI 构建后把 zip 产物与 `SHA256SUMS` 通过 `gh release create` 发布到这里。

## 为什么分成两个仓库

GitHub 私有仓库的 Release 资产**不允许匿名下载**。若让部署服务器带 Token 去拉私有产物，
等于每台服务器上都留了一份可读源码的凭据——而服务器恰是这类平台最可能被攻陷的资产。

分离后：源码私有、产物公开只读、服务器上**零凭据**。`selfupdate` 的三道校验
（域名白名单 → SHA256 比对 → 冒烟测试）不依赖仓库私有性，安全性不受影响。

## 目录约定

```
<tag>/  hengdun-<version>-<os>-<arch>.zip  +  SHA256SUMS
```

例如 `v2.4.0/hengdun-v2.4.0-linux-amd64.zip`。

## 发布流程（在源码仓库 CI 中执行）

1. `CaiWanq/hengdun` 打 tag（如 `v2.4.0`）触发 `.github/workflows/release.yml`；
2. CI 交叉编译并打包，生成 `SHA256SUMS`；
3. 用 `RELEASE_PAT`（仅对产物仓库有写权限）在此仓库创建 Release 并上传资产。

本仓库需保持非空（已有本 README），否则 `gh release create` 会因空仓库报 `HTTP 422`。
