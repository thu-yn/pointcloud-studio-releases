# PointCloud Studio 安装包与更新源

这个仓库只公开安装包、包管理器清单、APT 公钥和更新索引。应用源码继续保存在私有仓库，不在这里发布。

## 当前发布状态（2026-10-28）

0.4.1 安装包和签名 APT 源已上线，并验证了公网下载的内容与构建结果一致。Windows 安装/升级/保留项目卸载，以及干净 Ubuntu 容器里的 APT 安装/升级/程序启动和篡改索引拒绝，均已通过。

winget 上架申请：[PR #463900](https://github.com/microsoft/winget-pkgs/pull/463900)，目前尚未合并。微软机器人已要求账号本人阅读并确认 [CLA 贡献者协议](https://github.com/microsoft/winget-pkgs/pull/463900#issuecomment-6466297899)，之后仍需上游检查、审核和目录同步。现在可以用 APT 或直接下载 Windows 安装包；不能把 winget 已提交写成已经可搜索安装。

## Ubuntu 24.04 及更新版本（amd64）

首次添加更新源：

```bash
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://thu-yn.github.io/pointcloud-studio-releases/keys/pointcloud-studio.asc | sudo tee /etc/apt/keyrings/pointcloud-studio.asc >/dev/null
echo 'deb [arch=amd64 signed-by=/etc/apt/keyrings/pointcloud-studio.asc] https://thu-yn.github.io/pointcloud-studio-releases stable main' | sudo tee /etc/apt/sources.list.d/pointcloud-studio.list
sudo apt update
sudo apt install pointcloud-studio
```

以后更新：

```bash
sudo apt update
sudo apt install --only-upgrade pointcloud-studio
```

卸载应用使用 `sudo apt remove pointcloud-studio`，用户自行保存的项目不会随包删除。当前提供 Ubuntu 24.04 上构建的 amd64 包，Ubuntu 22.04、ARM 和 macOS 尚未提供对应包。

## Windows 10/11（x64）

从 [Releases](https://github.com/thu-yn/pointcloud-studio-releases/releases) 下载 `Setup.exe` 安装包，安装到当前用户目录，以后运行新版安装包即可升级。安装程序不会删除用户项目；程序运行中会拒绝覆盖，需关闭后重试。

包标识为 `ThuYn.PointCloudStudio`。**winget 公共目录是否已上架，以提交的 PR 和 `winget search --id ThuYn.PointCloudStudio --exact` 查询结果为准；清单上传到本仓库不等于已进入 winget 公共目录。** 审核通过并同步后可使用：

```powershell
winget install --id ThuYn.PointCloudStudio --exact --source winget
winget upgrade --id ThuYn.PointCloudStudio --exact --source winget
```

现有便携 ZIP 没有安装登记，首次需要运行安装包，之后才能由包管理器识别和升级。安装包未进行商业代码签名。应用仍需要可用的 OpenGL 3.3 显卡驱动，GPU 计算是可选项。

## 更新与校验

版本化安装包放在 Releases；APT 索引使用独立的 GPG 发布密钥签名，客户端通过 `signed-by` 指定公钥。保留历史版本的安装包和索引信息，已发布版本的文件不会用不同内容覆盖。`latest.json` 和 `manifests/` 保存当前版本与 winget 清单。

应用为专有软件，源码不在本仓库公开。安装包内包含第三方组件声明与相关许可文本。

winget 清单修订：原始清单压缩包缺少上游要求的 schema 注释头；请使用发布页的 `PointCloudStudio-0.4.1-winget-manifests-r2.zip` 或本仓库 `manifests/` 下的修正版。应用安装包及 APT 包保持原有内容。
