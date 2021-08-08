# PGP 与 GPG

PGP（aka Pretty Good Privacy）是一套用于加密和认证的工具，由菲尔·齐默曼开发。PGP 本身是商业软件，但菲尔·齐默曼于1997年与 IETF 制定了 OpenPGP 标准 [RFC2440](https://tools.ietf.org/html/rfc2440)（最新版本为 [RFC4880](https://tools.ietf.org/html/rfc4880)）。

自由软件基金会编写了他们的 OpenPGP 自由软件实现 GPG（GNU Privacy Guard, aka GnuPG），在事实上成为了 OpenPGP 系列软件的标准。

## GnuPG 的基本使用

### 安装 GPG

对于 Linux 用户，可以通过包管理器安装，一般名为 `gnupg`。

对于 macOS 用户，可以安装 GPGTool。（笔者对 macOS 不甚熟悉，需要补充和修正）

对于 Windows 用户，可以安装 [GPG4Win](https://www.gpg4win.org/)。

### 使用 GUI 工具

本节讲述如何使用 GPG 的 GUI 前端 Kleopatra。

Kleopatra 在 Windows 上作为 GPG4Win 的一部分被安装，在 Linux 上需自行从包管理器安装。

以下以英文版本为用例进行讲解，中文版本应很容易对应。

#### 生成密钥

首先，点击左上角的 <kbd>File</kbd>，随后在菜单中点击 <kbd>New Key Pair</kbd>，然后选择第一项“Create a personal OpenPGP key pair”，输入你选择的名字（一般建议使用 Nick Name 而不是真实姓名）和邮箱，如果需要设置可选（但建议）的密码的话，勾选“Protect the genterated key with a passphrase”。

如果需要更高级的配置，点击右下角的“Advanced Settings...”，笔者推荐将“Key Material”一栏中的“3,072 bits”改为“4,096 bits”以提高安全性。

点击右下角的 <kbd>Create</kbd>，随后输入两遍你想设置的密码（如未勾选设置密码则无此步），然后点击 <kbd>Finish</kbd>。

##### 生成子密钥

To Be Done（笔者不会）

#### 导出密钥

右键你想要导出的密钥，点击 <kbd>Backup secret keys</kbd> 即可。

#### 导入密钥

点击工具栏中的 <kbd>Import</kbd>，选择文件即可。

#### 文件加密

加密点击工具栏中的 <kbd>Sign/Encrypt</kbd>，选择文件，然后仅勾选 Encrypt，点击下一步即可。
解密点击工具栏中的 <kbd>Dncrypt/Verify</kbd>，选择文件，输入密码即可。

#### 签名与校验签名

同文件加密

### 使用命令行工具

本节讲述如何使用 GPG 的命令行工具。

#### 生成密钥

#### 导出密钥

#### 导入密钥

#### 文件加解密

#### 签名与校验签名

#### 为你的 Git Commit 签名
