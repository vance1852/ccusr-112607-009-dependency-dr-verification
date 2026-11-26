# 灾备演练的依赖隔离与恢复承诺验证

这里的设备、合作方及业务记录均为合成资料。两份记录分别从同一清册开始，可独立读取。记录包含申请、通知及实际观察，申请可能未获准。

## 资料字段

`records/inventory.json` 是初始清册。两份 `.jsonl` 文件均以每行一个 JSON 对象保存观察。`at` 是记录进入值班侧的演练秒，同一秒按原文件顺序。`kind` 标识记录类别。另有字段时，业务发生时间由记录自身给出。

plans 定义站点、能力使用的依赖、RTO 的秒数与最多允许的提交位置缺口。failure 以其 at 为计时起点，reference_position 作为损失计算的受约束基线。durable_at 是目的实例持有的应用断面，legacy 表示主站依赖。observed_call 由接收端上报实际去向。isolate 包含现场真实影响范围，owner 为仍依赖该连接的站点。

## 运行

在仓库根目录使用 Python 3.11 标准库。

```sh
python3 source_reader.py records/shift.jsonl
python3 source_reader.py records/comparison.jsonl
python3 -m unittest discover -s tests -v
```

读取工具按记录到达顺序显示原始资料，`--kind` 可按类别筛选。
