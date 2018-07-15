# 搭建Ruby环境
## 在Windows下安装Ruby 2.4.0以上版本

本节介绍如何在 Windows 环境下，使用 RubyInstaller 安装 Ruby 2.4.0 以上版本。

首先可以从以下网站下载 RubyInstaller。

+ <https://rubyinstaller.org/downloads/>

从页面可以发现，官方建议安装 Ruby+DevKit 的版本。
该版本在使用 gems 安装依赖 C 语言扩展的 Ruby 包时，能够直接对 Ruby 包进行编译，为用户带来更好的体验。
具体来说，所谓 DevKit 是基于 [MSYS2](http://www.msys2.org/) 的开发套件（Developement Kit），
通过在 Windows 下模拟 Linux 环境，使用 mingw-w64 等构建工具，编译所需的 C 语言扩展。

在页面中选择 WITH DEVKIT 下的安装包，即可下载包含开发套件的 Ruby 版本（主要在 Ruby 2.4.0后提供）。
此版本在安装 Ruby 后（GUI界面，记得更换路径），会启动控制台进行 DevKit 的安装。
一般来说，直接敲击回车，即可完成安装，默认的 DevKit 后被安装在 Ruby 所在文件夹下。
也可下载不包含 DevKit 的安装包（较小），在需要 DevKit 时再使用 ridk install 下载并安装 DevKit。
其中，第二种安装方式，需要自行选择 MSYS2 的安装位置。

然而，国内 MSYS2 源的速度较慢，可能需要等待很久才能完成 DevKit 的安装。
此时，可以终止安装，通过修改 MSYS2 源的方式，加快下载。

具体步骤如下：

1. 打开 MSYS2 目录，进入 /etc/pacman.d 文件夹。

2. 分别在 mirrorlist.mingw32、mirrorlist.mingw64、mirrorlist.msys 中添加源信息到最前面的行。
目前已知的速度较快的源如下所示。

  - | mirrorlist.mingw32 | 说明 |
    | - | - |
    | Server = http://mirrors.nju.edu.cn/msys2/mingw/i686 | 南京大学镜像 |
    | Server = http://mirrors.ustc.edu.cn/msys2/mingw/i686 | 中科大镜像 |
    | Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686 | 清华镜像 |
    | Server = https://sourceforge.net/projects/msys2/files/REPOS/MINGW/i686 | SF镜像，速度也很快 |

  - | mirrorlist.mingw64 |
    | - |
    | Server = http://mirrors.nju.edu.cn/msys2/mingw/x86_64 |
    | Server = http://mirrors.ustc.edu.cn/msys2/mingw/x86_64 |
    | Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64 |
    | Server = https://sourceforge.net/projects/msys2/files/REPOS/MINGW/x86_64 |

  - | mirrorlist.msys |
    | - |
    | Server = http://mirrors.nju.edu.cn/msys2/msys/$arch |
    | Server = http://mirrors.ustc.edu.cn/msys2/msys/$arch |
    | Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch |
    | Server = https://sourceforge.net/projects/msys2/files/REPOS/MSYS2/$arch/ |

3. 在控制台使用 ridk install 继续安装。
如果出现提示 Failed to init transaction (unable to lock database)，表示 pacman （msys2的包管理器）在更新数据库时被打断。
在确保没有其他 pacman 运行的情况下（留心其他控制台），可以删除 /var/lib/pacman/db.lck 并重新运行 ridk install。
