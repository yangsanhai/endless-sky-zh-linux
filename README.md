# Endless Sky · 无尽天空 · Linux 简体中文版

一个可以直接玩的 **Linux 简体中文版《Endless Sky》（无尽天空）**。
官方版本没有中文支持，本项目把社区中文数据、界面翻译、程序内文本全部整合好，Linux 用户开箱即用。

---

## 一、这是什么

- 游戏本体：Endless Sky `0.11.3-alpha`（基于 [SoluteToNight/endless-sky-CN](https://github.com/SoluteToNight/endless-sky-CN) 的 `feature/cjk-localization` 分支，用 FreeType + 思源黑体渲染中文）
- 中文数据：任务剧情、对话、星球描述、飞船/装备介绍等（来自该项目 Android 版的中文数据）
- 界面翻译：按钮、菜单、属性面板、提示消息、帮助弹窗、按键提示等（本项目补充，共 1456 条）
- 飞船/装备中文名：1170+ 条 `display name`
- 商品名、分类名：全部中文
- 完整图像与音效：官方原始素材，已一并打包

## 二、安装方式一：deb 包（推荐，双击安装）

到 [Releases](https://github.com/yangsanhai/endless-sky-zh-linux/releases) 下载 **`endless-sky-zh_0.11.3_amd64.deb`**：

```bash
# 用 apt 安装（会自动补依赖；改成你下载的实际路径）
sudo apt install ./endless-sky-zh_0.11.3_amd64.deb

# 或者
sudo dpkg -i endless-sky-zh_0.11.3_amd64.deb && sudo apt-get -f install
```

装完之后：

- 在应用菜单里搜 **「无尽天空」** 直接点开
- 或者终端里执行 `endless-sky-zh`

安装位置：程序与资源在 `/opt/endless-sky-zh/`，启动器 `/usr/games/endless-sky-zh`。
卸载：`sudo apt remove endless-sky-zh`。

## 三、安装方式二：AppImage（免安装，单文件）

下载 **`EndlessSkyZH-x86_64.AppImage`**，然后：

```bash
chmod +x EndlessSkyZH-x86_64.AppImage
./EndlessSkyZH-x86_64.AppImage
```

- 这个包**自带运行库**（SDL2/GLEW/OpenAL 等都在里面），换别的发行版基本也能直接跑
- 如果提示缺少 FUSE：Debian/Ubuntu 上 `sudo apt install libfuse2t64`（旧的叫 `libfuse2`），
  或者不解压直接跑：`./EndlessSkyZH-x86_64.AppImage --appimage-extract-and-run`
- 桌面环境里对 AppImage 双击也通常可直接运行（部分环境需先给执行权限）

## 四、从源码自己编译

```bash
sudo apt-get install -y build-essential cmake ninja-build git \
  libsdl2-dev libpng-dev libjpeg-dev libglew-dev libopenal-dev libmad0-dev \
  libavif-dev libflac++-dev libminizip-dev libfreetype-dev

# 取源码
git clone -b feature/cjk-localization https://github.com/SoluteToNight/endless-sky-CN.git src
cd src

# 套用界面翻译补丁
patch -p1 < ../interface-translation.patch

# 编译
cmake -S . -B build -G Ninja -DES_USE_VCPKG=OFF -DES_USE_SYSTEM_LIBRARIES=ON \
      -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF
cmake --build build -j$(nproc)

# 运行
./build/endless-sky
```

> 注意：运行时资源目录的识别方式是「从可执行文件所在目录开始，逐级向上找含有 `credits.txt` 的目录」。
> 所以**自己手动摆放文件时，`credits.txt` 必须和 `data/`、`images/`、`sounds/` 放在同一层**，否则游戏会报
> `找不到资源目录！` 直接退出。仓库根目录里那个 `endless-sky-linux-zh` 单独拿出来跑也会因此报错，建议直接用上面的 deb 或 AppImage。

## 五、仓库文件说明

| 文件/目录 | 说明 |
|---|---|
| `endless-sky-linux-zh` | 编译好的 Linux 可执行文件（64 位） |
| `data/` | 游戏数据（中文）：任务、对话、星球、飞船装备、商品等，含中文字体 |
| `credits.txt` | 游戏用来定位资源目录的锚点文件（**不能删**） |
| `images/`、`sounds/`、`shaders/` | 官方图像、音效与着色器素材 |
| `interface-translation.patch` | 界面文本的程序内注入补丁（1456 条，基于 0.11.3 源码） |
| `README.md` | 本文件 |

## 六、已知说明

- **地名/星系名/星球名保持英文**（地图地名，汉化意义不大）
- 存档位置：`~/.local/share/endless-sky/saves/`（安装/升级游戏不会动你的存档）
- 键位配置文件 `keys.txt` 里的动作名保持英文（翻译会导致按键失效，已刻意保留）
- 如果游戏里还有个别英文，欢迎提 Issue 指出来

## 七、许可与来源

- 本项目的游戏源码来自 [SoluteToNight/endless-sky-CN](https://github.com/SoluteToNight/endless-sky-CN)（GPL-3.0），
  上游为 [endless-sky/endless-sky](https://github.com/endless-sky/endless-sky)（GPL-3.0）。
- 中文数据来自该项目 Android 版（Endless Sky 中文数据）。
- 界面文本翻译参考了 [ok-komputer](https://github.com/ok-komputer) 提交的 PR（部分术语音译）。
- 本项目同样以 **GPL-3.0** 协议发布，保留原作者版权声明。

> Endless Sky 是 Michael Zahniser 等人的开源作品，本项目是非官方汉化，与官方无关。
