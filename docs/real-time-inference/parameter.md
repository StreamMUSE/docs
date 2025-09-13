# 参数指南

StreamMUSE Client 是一个实时音乐生成系统的客户端应用程序，支持多种输入模式并与 StreamMUSE 服务器通信生成音乐伴奏。

## 基本用法

```bash
python client.py [选项]
```

## 网络参数

### `--server_url`
- **类型**: 字符串
- **默认值**: `http://localhost:8988/generate_accompaniment`
- **说明**: StreamMUSE 服务器的 URL 地址
- **示例**: `--server_url http://192.168.1.100:8988/generate_accompaniment`

## 音乐时间参数

### `--tempo`
- **类型**: 浮点数
- **默认值**: `90.0`
- **说明**: 音乐速度，单位为 BPM (每分钟节拍数)
- **示例**: `--tempo 120`

### `--ticks_per_beat`
- **类型**: 整数
- **默认值**: `4`
- **说明**: 每个节拍包含的 tick 数量。在此系统中，1 tick = 1/4 beat
- **示例**: `--ticks_per_beat 8`

### `--beats_per_bar`
- **类型**: 整数
- **默认值**: `4`
- **说明**: 每小节包含的节拍数量
- **示例**: `--beats_per_bar 3` (3/4 拍子)

### `--generation_interval_ticks`
- **类型**: 整数
- **默认值**: `2`
- **说明**: 音乐生成请求之间的间隔，单位为 tick
- **示例**: `--generation_interval_ticks 4`

## 显示参数

### `--log_lines`
- **类型**: 整数
- **默认值**: `10`
- **说明**: 控制台显示的日志行数
- **示例**: `--log_lines 20`

### `--metronome`
- **类型**: 开关
- **默认值**: `False`
- **说明**: 启用可听见的 MIDI 节拍器声音
- **示例**: `--metronome`

## MIDI 输入输出参数

### `--midi_output_name`
- **类型**: 字符串
- **默认值**: `None` (自动选择)
- **说明**: 指定 MIDI 输出端口名称
- **示例**: `--midi_output_name "MIDI Out 1"`

### `--midi_input_name`
- **类型**: 字符串
- **默认值**: `None` (自动选择第一个可用端口)
- **说明**: 指定 MIDI 输入端口名称
- **示例**: `--midi_input_name "Digital Piano"`

### `--accompaniment-velocity`
- **类型**: 整数
- **默认值**: `50`
- **范围**: `0-127`
- **说明**: 生成的伴奏音符的 MIDI 力度值
- **示例**: `--accompaniment-velocity 80`

## 输入模式参数

### `--use-keyboard-input`
- **类型**: 开关
- **默认值**: `False`
- **说明**: 使用计算机键盘作为 MIDI 输入设备
- **示例**: `--use-keyboard-input`
- **键盘映射**:
  - 白键: `a s d f g h j k l`
  - 黑键: `w e t y u`

### `--midi-file-input`
- **类型**: 字符串
- **默认值**: `None`
- **说明**: 指定 MIDI 文件路径来模拟用户输入
- **示例**: `--midi-file-input path/to/song.mid`

### `--midi-file-delay-ticks`
- **类型**: 整数
- **默认值**: `0`
- **说明**: MIDI 文件开始播放前的延迟 tick 数
- **示例**: `--midi-file-delay-ticks 8`

### `--midi-file-use-original-duration`
- **类型**: 开关
- **默认值**: `False`
- **说明**: 使用 MIDI 文件中的原始音符时长，而不是固定时长
- **示例**: `--midi-file-use-original-duration`

## 音乐注入参数

### `--injection-file`
- **类型**: 字符串
- **默认值**: `None`
- **说明**: 要注入到推理引擎历史中的 MIDI 文件路径
- **要求**: 必须是旋律文件 (包含 "mel" 的路径)，系统会自动寻找对应的伴奏文件
- **示例**: `--injection-file data/prompts/prelude_mel.mid`

### `--injection-length`
- **类型**: 整数
- **默认值**: `0`
- **说明**: 从注入文件中注入的 tick 数量
- **要求**: 当指定 `--injection-file` 时必须为正数
- **示例**: `--injection-length 100`

## 使用示例

### 基本用法
```bash
# 使用默认 MIDI 设备
python client.py

# 使用键盘输入，120 BPM
python client.py --use-keyboard-input --tempo 120

# 使用 MIDI 文件输入，延迟 8 tick
python client.py --midi-file-input song.mid --midi-file-delay-ticks 8

# 启用节拍器和更高的伴奏力度
python client.py --metronome --accompaniment-velocity 80
```

### 音乐注入示例
```bash
# 注入前奏然后使用默认 MIDI 设备输入并启用节拍器
python client.py --injection-file prompt_mel.mid --injection-length 50 --use-keyboard-input --metronome

# 注入前奏然后使用键盘输入并启用节拍器
python client.py --injection-file prompt_mel.mid --injection-length 50 --use-keyboard-input --metronome

# 注入后使用 MIDI 文件继续并启用节拍器
python client.py --injection-file prompt_mel.mid --injection-length 100 --midi-file-input continuation.mid --metronome
```

### 高级配置
```bash
# 自定义节拍和生成间隔
python client.py --beats_per_bar 3 --ticks_per_beat 8 --generation_interval_ticks 4

# 指定特定的 MIDI 端口
python client.py --midi_input_name "Piano" --midi_output_name "Speakers"

# 连接到远程服务器
python client.py --server_url http://192.168.1.100:8988/generate_accompaniment
```

## 注意事项

1. **音乐注入**: 注入文件必须包含 "mel" (旋律) 和对应的 "acc" (伴奏) 文件
2. **时间单位**: 系统中 1 tick = 1/4 beat
3. **文件输出**: 每次会话都会在 `app/logs/session_TIMESTAMP/` 目录下生成日志文件
4. **键盘退出**: 使用 `Ctrl+C` 安全退出程序并保存所有会话数据

## 输出文件

程序运行结束后会在会话目录中生成：
- `performance.mid`: 完整的用户输入和模型生成的 MIDI 文件
- `prompt.mid`: 注入的 prompt MIDI 文件 (如果使用了注入功能)
- `inferences.json`: 详细的推理日志
- `session.csv`: 保存推理时间、latency 等数据的统计文件
- `session.txt`: 保存推理时间、latency 等数据的 summary