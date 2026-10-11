# MHR Item Injector 怪物猎人崛起：曙光 物品注入器

REFramework Lua 脚本。游戏内中文界面，按物品名搜索并一键把物品注入到**道具箱**（村庄家中 / 营地帐篷的箱子，从箱子取出即入随身背包）。

> 仅支持 PC（Steam）版《怪物猎人崛起：曙光》。

## 功能

- **中文界面**：1625 个物品的 ID→中文名对照表（含曙光全部物品），与 mhvuze 社区维基交叉校验
- **搜索注入**：搜索框输中文名（粘贴）或数字 ID → 下拉选择 → 数量滑条 → 一键注入
- **常用物品一键 x999**：回复药 / 秘药 / 鬼人药G / 强走药 / 闪光球 / 捕获用麻醉球 / 陷阱工具 / 各类弹药等 24 个
- **高级模式**：直接输入物品 ID 注入（配合 ID 表可发表内未收录物品）
- **实时状态自检**：窗口顶部常驻显示注入链路是否就绪；结果行显示游戏原始返回值（rv=true / false / nil），出问题一眼定位
- **API 自检导出**：一键把道具箱运行时方法表导出为 JSON，便于版本更新后重新适配
- **错误自显示**：UI 全程 pcall 保护，脚本异常直接显示在窗口里，不会静默消失
- 结果同步显示在游戏画面中央（绿色成功 / 红色失败）
- 配置持久化（记住上次选择的物品和数量）

## 安装

### 1. 安装 REFramework（已装跳过）

1. 到 [REFramework-nightly Releases](https://github.com/praydog/REFramework-nightly/releases) 下载最新 `REFramework.zip`（崛起是 DX11 游戏，用标准版，不要下 DX12 版）
2. 解压到游戏根目录（`MonsterHunterRise.exe` 所在目录），解压后应有 `dinput8.dll` 和 `reframework/` 文件夹

### 2. 安装本工具

把本仓库 `reframework/` 文件夹整个复制到游戏根目录，与 REFramework 的同名文件夹合并。合并后确认存在：

```
MonsterHunterRise\
├── dinput8.dll                    ← REFramework 的
└── reframework\
    ├── autorun\
    │   └── MHR_ItemInjector.lua   ← 本工具主脚本
    └── fonts\
        └── NotoSansSC-Regular.otf ← 中文字体（必须，否则界面中文变方块）
```

## 使用

1. 启动游戏，进入村庄或任务（标题菜单界面无法注入）
2. 按 **Insert** 打开 REFramework 菜单 → **Script Generated UI** → **MHR Item Injector**
3. 窗口顶部显示 `[OK] 道具箱已就绪` 后即可注入
4. 物品注入到**道具箱**：村庄/艾尔迦多的自己房间、任务中营地帐篷开箱查看；**怪物素材在"素材"页签**
5. 注入时若道具箱界面开着，关掉重开才会刷新

## 常见问题

| 现象 | 处理 |
|---|---|
| 搜索框打不出中文 | REFramework 输入框不支持中文输入法（框架已知限制）。游戏外复制中文名后 Ctrl+V 粘贴，或直接输数字 ID |
| 注入后"找不到物品" | 物品在道具箱不在随身袋；素材在"素材"页签；箱子开着需关掉重开 |
| 中文显示方块 | 字体没复制到位，确认 `reframework/fonts/NotoSansSC-Regular.otf` 存在 |
| 结果行显示 `rv=false` | 物品 ID 无效 / 对应页签满 / 箱子容量满，换物品或清理箱子 |
| 结果行显示 `[脚本错误]` | 把错误文字发 Issue，一看便知是哪个组件的问题 |

## 工作原理

注入调用链（社区实证）：

```
snow.data.DataManager
  → get_PlItemBox()
  → tryAddGameItem(snow.data.ContentsIdSystem.ItemId, System.Int32)
```

通过 REFramework 按游戏函数名调用，**不依赖内存偏移**，游戏小版本更新基本不受影响。UI 组件全部使用经实机验证存在的 REFramework Lua 绑定（特别注意：`imgui.input_int` 在部分版本不存在，会导致 UI 静默中断，勿用）。

## 致谢

- 物品注入调用链实证：[KrisCris/MHRise_Scripts](https://github.com/KrisCris/MHRise_Scripts)
- 物品 ID 中文对照：[Banson's Blog 全物品ID列表](https://banson.moe/posts/mhr-data-id-list/)
- ID 交叉校验：[mhvuze/MonsterHunterRiseModding Wiki](https://github.com/mhvuze/MonsterHunterRiseModding/wiki)
- 中文字体：[Noto Sans SC](https://fonts.google.com/noto/specimen/Noto+Sans+SC)（SIL OFL 1.1）
- UI 框架参考：[dzxrly/MHR-ItemSetAutoResupplyMod](https://github.com/dzxrly/MHR-ItemSetAutoResupplyMod)（MIT）
- 运行环境：[REFramework](https://github.com/praydog/REFramework) by praydog

## 声明

仅供单人游戏辅助使用；在公共集会所联机时请自律。使用本工具产生的存档变动、账号问题由使用者自行承担。

## License

[MIT](LICENSE) · 字体另按 SIL OFL 1.1 授权
