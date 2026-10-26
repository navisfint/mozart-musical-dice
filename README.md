# 莫扎特音乐骰子（Musikalisches Würfelspiel）

按莫扎特 K.516f 的规则掷骰子生成一首永不重复的小步舞曲——**掷两次骰子决定每个小节的乐句**，
16 个小节查表拼接，理论上有 `11^16 ≈ 4.6 × 10^16` 种组合。

## 玩法

```bash
pip install simpleaudio
python musical_dice_game.py
```

程序按骰子点数查表，从 272 个预置乐句（`.wav`）中挑出对应小节，
拼成完整曲目并播放。`musical_dice_game_play_only.py` 是纯播放版，不重新掷骰。

## 实现要点

**查表结构**：每个小节对应一组索引表，表的第 0、1 位是 `None`
——两个骰子的点数和范围是 2~12，永远不会掷出 0 或 1。用 `None` 占位比偏移量换算更不容易错。

```
minuet_table[16][13]   16 个小节 × 11 种乐句（索引 2~12 有效）
trio_table[N][13]      三声部段落，同样规则
```

**为什么用 `None` 而不是 `-1` 或补齐**：查表时代码直接 `minuet_table[bar][roll]`，
越界会立刻炸掉。用 `None` 占位，索引映射是字面的，读代码的人不需要在脑子里做一次减法。

## 目录

```
mozart-musical-dice/
├── musical_dice_game.py            掷骰 + 拼接 + 播放
├── musical_dice_game_play_only.py  只播放
└── samples/                        示例音频（完整曲目 + 单个乐句）
```

> 272 个乐句音频未包含在仓库内（体积原因）。
