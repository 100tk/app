## Variables

These variables are available for use in `pre_install` / `installer.script` / `post_install` / `pre_uninstall` / `uninstaller.script` / `post_uninstall` scripts:

| Variable        | Example                                      | Description      |
|-----------------|----------------------------------------------|------------------|
| **Non-path:**  | |
| `$app`          | `exampleapp`                                 | 应用程序的名称(JSON文件名)
| `$architecture` | `64bit` or `32bit`                           | 正在安装的应用程序的CPU架构
| `$cmd`          | `uninstall`, `update`, `install`             | 当前正在运行的子命令
| `$cfg`          | `{SCOOP_BRANCH, SCOOP_REPO, lastupdate, etc}`| Scoop configuration (powershell object)
| `$global`       | `$false` or `$true`                          | `$true` for global installs/uninstalls         
| `$manifest`     | `@{homepage=https://example.com/; description=Example app; version=2.4.1; url=http://example.com/app-setup.exe;...` | Deserialized manifest (powershell object) 
| `$version`      | `1.2.3`                                      | 正在安装的版本
| **Important Paths:**  | |
| `$dir`          | `C:\Users\username\scoop\apps\$app\current` <br/>`C:\ProgramData\scoop\apps\$app\current` (global installs) |
| `$persist_dir`  | `C:\Users\username\scoop\persist\$app`<br/>`C:\ProgramData\scoop\persist\$app` (global installs) |
| **Other Paths:**  | |
| `$bucketsdir`   | `C:\Users\username\scoop\buckets`            | 
| `$cachedir`     | `C:\Users\username\scoop\cache`              | 
| `$cfgpath`      | `~/.scoop`                                   | Path to Scoop configuration
| `$globaldir`    | `C:\ProgramData\scoop`                       | Typically `%ProgramData\scoop`, `%SCOOP_GLOBAL%` overrides)
| `$modulesdir`   | `C:\Users\username\scoop\modules`            |  
| `$original_dir` | `C:\Users\username\scoop\apps\$app\1.2.3`    | 
| `$oldscoopdir`  | `C:\Users\username\AppData\Local\scoop`      | 
| `$scoopdir`     | `C:\Users\username\scoop`                    | Base Scoop install dir (typically `%USERPROFILE%\scoop`, `%SCOOP% overrides)

(_check the [`lib/install`](https://github.com/ScoopInstaller/Scoop/blob/master/lib/install.ps1) script for more details_)

### The `$dir` variable

This variable expands to the path with the actual version string in `pre_install`, `pre_uninstall`, `post_uninstall`, `installer.script` and `uninstaller.script` fields. However, it expands to the path with the "current" name instead of version in the `post_install` field.

## Functions

### `appdir`

Reference another scoop application. Eg, to check if another application is installed you can use:

`"post_install": [ "if (Test-Path \"$(appdir otherapp)\\current\\otherapp.exe\") { <# .. do something .. #> }"`

