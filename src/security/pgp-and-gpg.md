# PGP 与 GPG

PGP（aka Pretty Good Privacy）是一套用于加密和认证的工具，由菲尔·齐默曼（Phil Zimmermann）开发。PGP 本身是商业软件，但菲尔·齐默曼于1997年与 IETF 制定了 OpenPGP 标准 [RFC2440](https://tools.ietf.org/html/rfc2440)（最新版本为 [RFC4880](https://tools.ietf.org/html/rfc4880)）。

> 1991年，菲尔·齐默曼写了第一个个人可以用的高强度加密软件 PGP，PGP 使用的密钥长度大于128位，远远超过了美国政府的管制规定。之后他把源代码放到互联网上让人们随便下载。当美国之外也有人下载的时候，这件事就惊动了美国政府，美国政府开始对他进行调查。齐默曼决定利用美国宪法第一修正案来保护 PGP 的自由发行，他通过 MIT 出版社出版了一本书，书的内容就是 PGP 的全部源代码，这样，PGP 的源代码就成为了受美国宪法第一修正案保护的言论自由。人们开始通过这种手段对抗管制。最终，在新世纪到来之后，这类案件分别在第六巡回上诉法庭和第九巡回上诉法庭得到了同样的判决：软件源代码是言论自由，受宪法第一修正案的保护。自此，美国政府再也不能试图限制软件源代码流通了

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

在终端中输入 `gpg --full-generate-key` 来生成一对密钥，GPG 会逐个询问每个配置项。

首先会询问你想要的密钥类型：
```
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 
```
关于各加密方式的实现和安全性，可以自行查阅资料，这里不再赘述。   
对于我们的一般需求选择默认的 `(1) RSA and RSA (default)` 就好。

接下来，GPG 会询问密钥长度：
```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 
```
可选 1024 至 4096 的任意一个值，对 RSA 默认值为 3072，为了更好的更长久的安全性，这里推荐设置成 4096。

然后会询问你密钥的有效期限：
```
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 
```
默认是密钥永不过期，但是，**极度不建议使用永不过期**，这不是一个好的安全实践。

笔者推荐设置为两年，并设置日历提醒自己在有效期限临近的时候[重新设置有效期限](#设置有效期限)。

键入 `2y`，回车，`y`，然后再回车。

输入你的用户名和邮件地址还有注释（如果不想填可直接回车留空）。

然后设置密码。

最后得到的密钥如下：
```
pub   rsa4096/39156228415722AD 2021-08-08 [SC] [expires: 2023-08-08]
uid                              Your Name (For intro to GPG) <mail@example.org>
sub   rsa4096/F0BB252EA3DF854B 2021-08-08 [E] [expires: 2023-08-08]
```

##### 生成子密钥

首先使用 `gpg --list-keys` 查看所有的 key，`rsa4096/` 后面的40位16进制数就是密钥的指纹（fingerprint）。

可以看到，GPG 在创建主密钥的过程中已经为我们创建了一个用于加密的子密钥。

然后，执行 `gpg --edit-key <fingerprint>`，注意将 `<fingerprint>` 换为你自己的密钥指纹，以下同理。

然后执行 `addkey`，根据用途自行选择，注意选 RSA 的。

最后执行 `save`。

#### 导出密钥

如果要导出公钥，执行
```console
$ gpg --armor --output <filename> --export <uid>
```
`<filename>` 是导出的文件的名称，`<uid>` 可以是你的姓名，邮箱，密钥指纹，只要能惟一确定密钥即可。注意如果导出子密钥的话，使用密钥指纹需要在后面加个感叹号。

如果要导出私钥，主密钥将 `--export` 替换为 `--export-secret-keys`，子密钥替换为 `--export-secret-subkeys` 即可。

<keyserver, To Be Done>

#### 导入密钥

公钥的话 `gpg --import <filename>` 即可，私钥用 `gpg --allow-secret-key-import --import <filename>`。

<keyserver, To Be Done>

#### 文件加解密

加密：
```console
$ gpg --recipient <uid> --output example.txt.gpg --encrypt example.txt
```
uid 意义同上文，将 `example.txt` 使用 uid 指定的密钥加密，保存到 `example.txt.gpg`。

解密：
```console
$ gpg --output example.txt --decrypt example.txt.gpg
```
将 `example.txt.gpg` 解密，保存到 `example.txt`。

#### 签名与校验签名

对文件签名：
```console
$ gpg --armor --detach-sign example.txt
```
会在同目录下生成一个 `example.txt.asc` 签名文件（纯 ASCII），如果不加 `--armor` 则生成的是二进制文件 `example.txt.sig`

验证签名：
将文件和签名文件放在同一目录，然后执行：
```console
$ gpg --verify example.txt.sig example.txt
```

签名消息:
执行 `gpg --clear-sign`，输入消息，然后 Ctrl+D 结束输入。

#### 为你的 Git Commit 签名

只需对 Git 进行如下配置即可：
```console
$ git config --global user.signingkey <uid>
```

然后 commit 的时候加 `-S` 就可以签名了，如果想要每次 commit 都自动签名，执行：
```console
$ git config --global commit.gpgsign true
```

在 GitHub 的设置里上传你的 GPG 公钥就能获得小绿勾啦。

#### 设置有效期限

执行 `gpg --edit-key <fingerprint>`，注意将 `<fingerprint>` 换为你自己的密钥指纹。

如果要修改子密钥的，需要先手动执行 `key <fingerprint>` 切换到对应 key.

然后执行 `expire`，选择期限，执行 `save`。

#### 生成吊销证书与吊销密钥

```console
$ gpg --output master-key.rev --gen-revoke <uid>
```
注意只能导出主密钥的吊销证书。此外gpg默认已经为我们生成了一份吊销证书，位于 `$GPGHOME/openpgp-revocs.d/`。

如要吊销主密钥，执行 `gpg --import master-key.rev` 即可。

如要吊销子密钥，执行 `gpg --edit-key <fingerprint>`，然后执行 `key <fingerprint>` 切换到对应 key，执行 `revkey`。
