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

## 二、怎么直接用（推荐）

仓库根目录的 `endless-sky-linux-zh` 就是编译好的 Linux 可执行文件。

```bash
# 1. 下载整个仓库（或 git clone）
git clone https://github.com/yangsanhai/endless-sky-zh-linux.git
cd endless-sky-zh-linux

# 2. 安装运行库（Debian/Ubuntu）
sudo apt-get install -y libsdl2-2.0-0 libpng16-16 libjpeg62-turbo libglew2.2 \
  libopenal1 libmad0 libavif16 libflac++10 libminizip1t64 zlib1g libfreetype6

# 3. 在仓库目录里运行（必须在这个目录，因为要读 data/ 和 fonts/）
chmod +x endless-sky-linux-zh
./endless-sky-linux-zh
```

> 注意：**必须在仓库根目录运行**，否则读不到 `data/` 和 `fonts/`。

## 三、怎么自己编译

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

## 四、文件说明

| 文件/目录 | 说明 |
|---|---|
| `endless-sky-linux-zh` | 编译好的 Linux 可执行文件（64 位） |
| `data/` | 游戏数据（中文）：任务、对话、星球、飞船装备、商品等 |
| `fonts/` | 中文字体（思源黑体 SourceHanSansCN） |
| `interface-translation.patch` | 界面文本的程序内注入补丁（1456 条，基于 0.11.3 源码） |
| `README.md` | 本文件 |

## 五、已知说明

- **地名/星系名/星球名保持英文**（地图地名，汉化意义不大）
- 存档位置：`~/.local/share/endless-sky/`
- 键位配置文件 `keys.txt` 里的动作名保持英文（翻译会导致按键失效，已刻意保留）
- 如果游戏里还有个别英文，欢迎提 Issue 指出来

## 六、许可与来源

- 本项目的游戏源码来自 [SoluteToNight/endless-sky-CN](https://github.com/SoluteToNight/endless-sky-CN)（GPL-3.0），
  上游为 [endless-sky/endless-sky](https://github.com/endless-sky/endless-sky)（GPL-3.0）。
- 中文数据来自该项目 Android 版（Endless Sky 中文数据）。
- 界面文本翻译参考了 [ok-komputer](https://github.com/ok-komputer) 提交的 PR（部分术语音译）。
- 本项目同样以 **GPL-3.0** 协议发布，保留原作者版权声明。

> Endless Sky 是 Michael Zahniser 等人的开源作品，本项目是非官方汉化，与官方无关。
