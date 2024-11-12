应用程序清单是描述如何安装程序的JSON文件。

**示例**:

```json
{
    "version": "1.0",
    "url": "https://github.com/lukesampson/cowsay-psh/archive/master.zip",
    "extract_dir": "cowsay-psh-master",
    "bin": "cowsay.ps1"
}
```

当此清单与 `scoop install` 命令一起运行时，它将下载由 `url` 指定的ZIP文件，从ZIP文件中提取 "cowsay-psh-master" 目录，然后使 `cowsay.ps1` 脚本在您的路径上可用。

更多示例，请参见 [主要Scoop存储桶](https://github.com/ScoopInstaller/Main/tree/master/bucket) 中的应用程序清单。

### 必需属性

**version**: 此清单安装的应用程序版本。

**description**: 包含程序简短描述的一行字符串。如果程序名称与应用程序的文件名相同，则不要包含程序名称。虽然这不是技术上的必需项，但所有新的或更新的清单都应包含描述。

**homepage**: 程序的主页。

**license**: 程序的软件许可证字符串或哈希。对于知名许可证，请使用 <https://spdx.org/licenses> 中找到的标识符。对于其他许可证，如果可用，请使用许可证的URL。否则，使用“Freeware”、“Proprietary”、“Public Domain”、“Shareware”或“Unknown”（定义如下）。如果不同文件有不同的许可证，用逗号（,）分隔许可证。如果整个应用程序是[双重许可](https://en.wikipedia.org/wiki/Multi-licensing)，用竖线符号（|）分隔许可证。
  - `identifier`: SPDX标识符，或“Freeware”（永远免费使用）、“Proprietary”（必须付费使用）、“Public Domain”、“Shareware”（免费试用，最终必须付费）或“Unknown”（无法确定许可证），根据情况选择。
  - `url`: 对于非SPDX许可证，包括指向许可证的链接。也可以包括指向SPDX许可证的链接。
  - 如果难以确定所有不同的许可证，可以在SPDX列表末尾添加 `,...`。如果有开源和非开源许可证，请先列出所有非开源许可证（例如，Freeware/Proprietary/Shareware）。
  - 如果无法确定应用程序的许可证，使用 "Unknown"。

### 可选属性

**##**: 包含注释的单行字符串或字符串数组。

**url**: 要下载的文件的URL或URLs。如果有多个URL，可以使用JSON数组，例如 `"url": [ "http://example.org/program.zip", "http://example.org/dependencies.zip" ]`。URL可以是HTTP、HTTPS或FTP。
  - 要更改下载URL的文件名，可以在URL后附加URL片段（以 `#` 开头）。例如，
  - `"http://example.org/program.exe"` -> `"http://example.org/program.exe#/dl.7z"`
  - 注意片段必须以 `#/` 开头才能生效。
  - 在上述示例中，Scoop 将下载 `program.exe` 但保存为 `dl.7z`，然后使用7-Zip自动解压。此技术常用于Scoop清单，以绕过可能有不良副作用的可执行安装程序，如注册表更改、安装目录外的文件放置或管理员提升提示。

**architecture**: 如果应用程序不是32位的，可以使用架构来区分([示例](https://github.com/ScoopInstaller/Main/blob/master/bucket/7zip.json))。

  - `32bit|64bit|arm64`: 包含特定架构的说明 (`bin`, `checkver`, `extract_dir`, `hash`, `installer`,  `pre_install`, `post_install`, `shortcuts`, `uninstaller`, `url`)。

**autoupdate**: 定义如何自动更新清单。

**bin**: 要添加到用户路径中的程序（可执行文件或脚本）。还可以创建一个别名，使用不同的名称调用实际的可执行文件，并（可选）传递参数给可执行文件。
    例如，使用 `[ "program.exe", "alias", "--arguments" ]`。
    参见 [busybox](https://github.com/ScoopInstaller/Main/blob/master/bucket/busybox.json) 示例。
    但是，如果只声明一个这样的别名，必须确保它被包含在一个外部数组中，例如：`"bin": [ [ "program.exe", "alias" ] ]`。否则它将被视为单独的别名。

**checkver**: 应用程序维护者和开发者可以使用 [bin/checkver](https://github.com/lukesampson/scoop/blob/master/bin/checkver.ps1) 工具检查应用程序的新版本。
清单中的 `checkver` 属性是一个正则表达式，可用于从应用程序主页匹配当前稳定版本。
例如，参见 [go](https://github.com/ScoopInstaller/Main/blob/master/bucket/go.json) 清单。
如果主页没有可靠的当前版本指示，还可以指定一个不同的URL进行检查。
例如，参见 [ruby](https://github.com/ScoopInstaller/Main/blob/master/bucket/ruby.json) 清单。


**env_add_path**: 将此目录添加到用户的路径（如果使用 `--global` 则添加到系统路径）。目录相对于安装目录，并且必须位于安装目录内。

**env_set**: 为用户（如果使用 `--global` 则为系统）设置一个或多个环境变量（[示例](https://github.com/ScoopInstaller/Main/blob/master/bucket/go.json)）。

**extract_dir**: 如果 `url` 指向压缩文件（支持 .zip, .7z, .tar, .gz, .lzma, .lzh），Scoop 将仅提取指定的目录。

**extract_to**: 如果 `url` 指向压缩文件（支持 .zip, .7z, .tar, .gz, .lzma, .lzh），Scoop 将提取所有内容到指定的目录（[示例](https://github.com/lukesampson/scoop-extras/blob/master/bucket/irfanview.json)）。

**hash**: 每个 `url` 对应一个文件哈希。默认使用 SHA256，但可以通过前缀 `sha512:`, `sha1:` 或 `md5:` 使用 SHA512, SHA1 或 MD5。

**innosetup**: 如果安装程序基于 InnoSetup，设置为布尔值 `true`（不加引号）。

**installer**: 非MSI安装程序的运行指令。
  - `file`: 原生安装方式，安装程序可执行文件路径。对于 `installer` 默认为最后一个下载的URL。对于 `uninstaller` 必须指定。

  - `script`: 脚本方式安装，powershell或cmd批处理。

  - `args`: （可选）传递给安装程序的参数数组。

  - `keep`: （可选）如果安装程序在运行后需要保留（例如用于未来的卸载），设置为 `"true"`。如果省略或设置为其他值，安装程序将在运行后删除。参见 [`extras/oraclejdk`](https://github.com/lukesampson/scoop-extras/blob/master/oraclejdk.json) 示例。此选项在 `uninstaller` 指令中将被忽略。

  - 可用变量：`$fname`（最后下载的文件），`$manifest`（反序列化的清单引用），`$architecture`（`64bit` 或 `32bit`），`$dir`（安装目录）

  - 在 scoop install 和 scoop update 期间调用。

**uninstaller**: 卸载脚本。在 scoop uninstall 和 scoop update 期间调用。

**pre_install**: 预安装脚本，程序安装前执行。

**pre_uninstall**: 预卸载脚本，程序卸载前执行。

**post_install**: 程序安装后执行脚本。这些命令可以使用 `$dir`, `$persist_dir`, 和 `$version` 等变量。

**post_uninstall**: 后卸载脚本，程序卸载后执行。

**notes**: 安装应用程序后显示的消息。

**persist**: 应用程序数据目录。

**psmodule**: 在 `~/scoop/modules` 中安装为PowerShell模块。
  - `name`（`psmodule` 所需）：模块的名称，应至少与提取目录中的一个文件匹配，以便PowerShell识别此为模块。

**shortcuts**: 开始菜单快捷方式。参见 [sourcetree](https://github.com/lukesampson/scoop-extras/blob/master/bucket/sourcetree.json) 示例。数组必须包含可执行文件/标签对。第三和第四个元素是可选的。
  1. 目标文件路径 [必需]
  2. 快捷方式名称（支持子目录：`<AppsSubDir>\\<AppShortcut>` 例如 [sysinternals](https://github.com/lukesampson/scoop-extras/blob/master/bucket/sysinternals.json)） [必需]
  3. 启动参数 [可选]
  4. 图标文件路径 [可选]

**depends**: 应用程序运行依赖，将自动安装。

**suggest**: 作为 `depends` 的替代方案。显示建议安装的可选应用程序的消息，这些应用程序提供补充功能。
  参见 [ant](https://github.com/ScoopInstaller/Main/blob/master/bucket/ant.json) 示例。
  - `["Feature Name"] = [ "app1", "app2"... ]`<br>例如，`"JDK": [ "extras/oraclejdk", "openjdk" ]`<br>
  如果任何建议的特征应用程序已安装，该特征将被视为“满足”，用户不会看到任何建议。



### 未记录的属性

- `cookie`: 仅在此处找到 [here](https://github.com/se35710/scoop-java/search?q=cookie&unscoped_q=cookie)

### 已弃用的属性

**此属性已弃用，未来版本的Scoop将移除支持。**- *新方法是将 .msi 文件视为 .zip 文件，从中提取文件而不运行完整安装。只需不在清单中包含此 `msi` 属性即可使用新方法。*
  - `code` *必需*: MSI安装程序的产品代码GUID
  - `silent`: 通常应为 `true` 以尝试无弹出窗口和UAC提示安装
