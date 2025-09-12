# 常见问题

## 一般问题

### Q: StreamMUSE 支持哪些模型格式？
A: 目前支持 PyTorch (.pth, .pt)、TensorFlow (.pb, SavedModel)、ONNX (.onnx) 等格式。

### Q: 如何监控系统性能？
A: 系统内置 Prometheus + Grafana 监控栈，可以实时查看性能指标。访问 `http://localhost:3000` 查看监控面板。

### Q: 支持多GPU训练吗？
A: 是的，Training Framework 支持多GPU和分布式训练。

## Real-Time Inference System

### Q: 推理延迟是多少？
A: 典型延迟在 10-50ms 之间，具体取决于模型大小和硬件配置。

### Q: 如何优化推理性能？
A: 可以通过以下方式优化：
- 启用模型量化
- 使用 GPU 加速
- 调整批处理大小
- 启用缓存

### Q: 如何处理并发请求？
A: 系统支持自动负载均衡和动态扩缩容，可以根据负载自动调整服务实例数量。

## Training Framework

### Q: 如何开始一个新的训练任务？
A: 使用以下命令：
```bash
streammuse-training run --config config.yaml
```

### Q: 训练可以断点续传吗？
A: 是的，系统自动保存检查点，可以从任意检查点恢复训练。

### Q: 如何查看训练日志？
A: 访问 MLflow UI (`http://localhost:5000`) 查看详细的训练日志和指标。

## 故障排除

### 服务无法启动
1. 检查端口是否被占用
2. 确认依赖服务是否正常运行
3. 查看日志文件获取详细错误信息

### 内存不足
1. 减少批处理大小
2. 启用梯度累积
3. 使用模型并行

### GPU 相关问题
1. 确认 CUDA 版本兼容性
2. 检查 GPU 内存使用情况
3. 验证 NVIDIA 驱动程序
