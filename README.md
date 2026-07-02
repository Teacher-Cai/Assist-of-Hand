![pic](https://github.com/Teacher-Cai/Assis-of-Hand/blob/main/asset/fJ2daHTQw.jpeg)

我们在日常生活中常会遇到一些重复性点击的工作，比如在某个网页上下载一小批文件，各种社交软件信息的清理，还有一些抢票类的事情。我们当前的信息技术水平已经发展到空前的高度，AI 都具有了一定的智能性了，所以这些重复性的点击操作也应该有一个软件协助我们操作。

基于这个初衷，我做了一个小软件来解决这个重复点击的问题。软件的核心思路是：把"在哪个坐标、点哪个键、按多久、隔多久"这几件事配置成一组步骤，然后让程序自动循环执行。基于这个软件的功能，我给软件取名叫做**手的助手**。

In our daily life, we often encounter repetitive clicking tasks, such as downloading a small batch of files on a certain webpage, cleaning up various social software information, and some ticket-grabbing activities. Our current information technology has developed to an unprecedented height, and AI has already achieved a certain level of intelligence. Therefore, there should be a software to assist us in these repetitive clicking operations.

Based on this idea, I developed a small software to solve the problem of repetitive clicking. The core idea is: configure "which coordinate, which button, how long to press, how long to wait" as a set of steps, and let the program automatically execute them in a loop. Based on the functions of this software, I named it **Assist of Hand**.

---

## 功能特性 Features

- **可视化步骤配置**：通过图形界面按序号逐条添加点击步骤，无需手写脚本。
- **多种点击方式**：支持鼠标左键 / 右键，支持短按（单击）与长按（按住指定时长再松开）。
- **实时坐标显示**：界面右上角实时显示当前鼠标位置，按 `Enter` 可一键将当前位置填入"点击位置"输入框。
- **灵活的时间控制**：每一步可独立设置点击前 / 点击后的等待时长（秒，支持小数）。
- **循环执行**：可设置循环次数，适合批量处理场景。
- **测试一次**：提供"测试一次"按钮，只跑一轮不循环，方便先验证配置是否正确。
- **安全机制**：执行过程中若鼠标位置发生移动（用户接管），程序会自动停止，避免误操作。
- **配置自动保存**：关闭窗口时自动将步骤配置与循环次数写入本地 `assist_of_hand.config`，下次启动自动恢复。
- **快捷键**：
  - `Enter` — 将当前鼠标坐标填入"点击位置"
  - `Esc` — 退出程序

---

## 界面预览 Preview

![界面示意](https://github.com/Teacher-Cai/Assis-of-Hand/blob/main/asset/fJ2daHTQw.jpeg)

界面分三个区域：
- **左侧** —— 执行配置信息（以字典形式展示，可手动编辑，会实时校验格式）。
- **右上** —— 步骤录入区：序号、左右键、点击位置、短按/长按、等待时长，以及"配置写入"按钮。
- **底部** —— 循环次数、测试一次、运行三个按钮。

---

## 环境要求 Requirements

- Python 3.x
- 依赖：
  - `pymouse`（用于模拟鼠标点击与获取坐标）
  - `tkinter`（Python 自带，无需额外安装）

> ⚠️ Windows 上 `pymouse` 依赖 PyHook / `pywin32` 等底层库，若安装过程出现问题，请参考 `pymouse` 的官方安装说明。

## 安装与运行 Install & Run

```bash
# 1. 克隆仓库
git clone https://github.com/Teacher-Cai/Assis-of-Hand.git
cd Assis-of-Hand

# 2. （建议）创建虚拟环境
python -m venv venv
# Windows 激活
venv\Scripts\activate

# 3. 安装依赖
pip install pymouse

# 4. 启动程序
python main.py
```

---

## 使用方法 Usage

1. 启动 `main.py`，窗口打开后，将鼠标移到目标位置，按 `Enter` 把坐标写入"点击位置"。
2. 选择鼠标左键 / 右键、短按 / 长按、设置等待时长，点击 **配置写入**，步骤 1 就写好了；序号会自动 +1，继续添加步骤 2。
3. 在底部"循环次数"中填入要循环的轮数。
4. 点击 **测试一次** 先跑一轮验证；没问题再点击 **运行** 正式执行。
5. 关闭窗口时，配置会自动保存；下次打开会恢复到上次的状态。
6. 运行中如需紧急停止，把鼠标移开即可，程序检测到鼠标位置变化会自动中断。

### 配置格式 Config Format

左侧文本框中保存的是一段 Python 字典，例如：

```python
{
    '1': {'location': '520,350', 'wait_sec': 1.0, 'left_or_right': 1, 'short_or_long': 0},
    '2': {'location': '800,200', 'wait_sec': 0.5, 'left_or_right': 1, 'short_or_long': 0},
    '3': {'location': '300,400', 'wait_sec': 2.0, 'left_or_right': 2, 'short_or_long': 1},
}
```

字段说明：

| 字段 | 含义 | 取值 |
| --- | --- | --- |
| `location` | 屏幕坐标 `"x,y"` | 字符串，整数 |
| `wait_sec` | 该步骤点击前 / 后的等待时长（秒） | 浮点数 |
| `left_or_right` | 鼠标按键 | `1` 左键 / `2` 右键 |
| `short_or_long` | 点击方式 | `0` 短按 / `1` 长按 |

键名 `'1' / '2' / ...` 即步骤的执行顺序，程序按数字升序依次执行。

---

## 项目结构 Project Structure

```
assist-of-hand/
├── main.py            # 程序入口：初始化全局状态、加载配置、启动 GUI
├── gui.py             # Tkinter 界面布局与事件绑定
├── key_func.py        # 点击核心逻辑：短按 / 长按、循环执行、单轮执行
├── utils.py           # 工具函数：鼠标位置监听、配置读写、自定义文本控件
├── global_info.py     # 全局状态（鼠标对象、配置、循环次数等）
├── asset/             # 图片与图标资源
│   ├── ccc.ico
│   └── fJ2daHTQw.jpeg
├── LICENSE
├── README.md
└── .gitignore
```

各模块职责：

- **main.py** —— 启动时创建 `PyMouse` 实例、读取本地持久化配置、启动 GUI。
- **gui.py** —— 构建 Tkinter 界面；绑定"配置写入"、"测试一次"、"运行"按钮；监听 `Enter` / `Esc`。
- **key_func.py** —— `short_click` / `long_click` 实现单次点击；`do` 按循环次数执行所有步骤；`do_for_one` 仅跑一轮。
- **utils.py** —— `where_is_cursor` 持续刷新鼠标位置；`detect_mouse_move_to_break` 安全中断检测；`load_config` / `write_config` 持久化；`CustomText` 让 Text 控件支持 `<<TextModified>>` 事件以便实时校验。
- **global_info.py** —— 跨模块共享的全局变量（`config`、`loop_times`、`iMouse` 等）。

---

## 注意事项 Notes

- 本工具通过模拟鼠标点击实现自动化，请仅用于个人效率提升与合法场景，**不要用于违反目标服务条款或法律法规的用途**（例如恶意刷单、攻击他人账号等）。
- 屏幕坐标系以系统主显示器左上角为原点 `(0, 0)`，多显示器环境请自行确认坐标正确性。
- 程序运行期间窗口会自动最小化（`iconify`），这是为了避免遮挡目标区域。配置完成后直接执行即可。

---

## 许可 License

本项目基于 [MIT License](LICENSE) 开源。
