# 快速开始

本指南将帮助您快速部署和运行 Real-Time Inference System。

## 前置要求

- UV 包管理工具

## 安装

### 克隆仓库

```bash
git clone https://github.com/AHY123/StreamMUSE.git
```

## 启动服务

本系统有 Server 端和 Client 端，所以需要运行两个程序。

### Server 端

一下操作都在 Server 端的 terminal 内进行。

#### 设置环境变量

首先我们需要设置必须的环境变量：

+ `CHECKPOINT_PATH` 设置为你的模型 checkpoint 路径

+ `MODEL_MAX_SEQ_LEN_FRAMES` 设置为 inference window 的长度，也就是模型最多可以看到多久的音乐（单位是 tick）

+ `GENERATION_LENGTH_FRAMES` 设置为每次 inference 生成的长度（单位是 tick）

```bash
export CHECKPOINT_PATH=results/Baseline/cp_transformer_909+ac+1k7_trackemb_interleavepos_v0.2_large_batch_40_schedule.epoch=00.val_loss=0.90296.ckpt
export MODEL_MAX_SEQ_LEN_FRAMES=384
export GENERATION_LENGTH_FRAMES=6
```

#### 启动 Server 端程序

```bash
uv run -- uvicorn app.server:app --host 0.0.0.0 --port 8988
```

第一次运行 `uv run` 的时候，UV 包管理工具会自动下载需要下载的包。

### Port Forwarding

当前版本是使用最简单的方法进行 Server 端与 Client 端的通讯的，我们用 SSH 把 Server 端的对应端口映射到 Client 这边的一个指定端口上，然后正常运行程序。

在你的 Server terminal 上，运行一下代码：

```bash
ssh -L 8000:localhost:8988 user@server_ip
```

`user@server_ip` 换成你对应的 username 和 server ip。

#### Notes

需要注意的是，在做 port forwarding 的时候，有的时候我们 Server 端的程序并没有绑在 localhost 端口上，如果按照上面的方法尝试运行失败了，试一下
`hostname -i` 命令，看一下 Server 端机器对应的 IP，然后把 localhost 换成这个 IP。

### Client 端

以下操作都在 Client terminal 内完成。

#### Set Up Synthesizer

本项目我们用的 soundfont 文件是 FluidR3_GM.sf2，所以需要先下载这个文件。

下在完成后运行以下命令，来打开运行 fluidsynth。

```bash
fluidsynth 'PathToYourFluidR3_GMFile/FluidR3_GM.sf2'
```

#### 启动 Client 端程序

Client 端有很多参数可以选择，我这里给出几个常用的例子：

```bash
PYTHONPATH="$(pwd)" uv run app/client.py --use-keyboard-input --metronome
```

这行命令表示你使用 computer keyboard 进行输入，同时打开了 metronome。

```bash
PYTHONPATH="$(pwd)" uv run app/client.py --injection-file input/mel/001.mid --injection-length 75 --metronome
```

这行命令表示你使用了 midi input（默认），同时 inject 了一首歌，inject 的长度是 75 ticks，打开了 metronome。

#### Notes

你在运行 Client 程序的时候，如果你的系统正确识别了相应的 port，程序会把相应的 port 打印出来，你可能需要人工选择 input port 用哪个，output port 用哪个，
机器自动 load 可能会出错。详情可以查看 [参数指南](parameter.md)。

## 测试API

```bash
curl -X POST http://localhost:8080/predict \
  -H "Content-Type: application/json" \
  -d '{"input": "your input data"}'
```

## 下一步

- 查看 [参数指南](parameter.md) 了解更详细的参数配置
- 查看 [配置指南](configuration.md) 了解详细配置选项
- 查看 [API 参考](api-reference.md) 了解完整的API文档
