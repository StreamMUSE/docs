# 环境设置

## 系统要求

- Python 3.8+
- CUDA 11.0+ (GPU训练)
- Docker (推荐)
- 至少 16GB RAM

## 安装步骤

### 1. 克隆仓库

```bash
git clone https://github.com/StreamMUSE/training.git
cd training
```

### 2. 创建虚拟环境

```bash
conda create -n streammuse-training python=3.9
conda activate streammuse-training
```

### 3. 安装依赖

```bash
pip install -r requirements.txt
pip install -e .
```

### 4. 配置环境

创建 `.env` 文件：

```bash
# 数据库配置
DB_HOST=localhost
DB_PORT=5432
DB_NAME=streammuse
DB_USER=admin
DB_PASSWORD=password

# MLflow 配置
MLFLOW_TRACKING_URI=http://localhost:5000

# 存储配置
DATA_ROOT=/path/to/data
MODEL_ROOT=/path/to/models
```

### 5. 启动服务

```bash
# 启动数据库
docker-compose up -d postgres

# 启动 MLflow
mlflow server --host 0.0.0.0 --port 5000

# 启动训练服务
streammuse-training serve
```

## 验证安装

```bash
streammuse-training --version
streammuse-training test-connection
```

## Docker 部署

```bash
docker-compose up -d
```

这将启动所有必需的服务：
- PostgreSQL 数据库
- MLflow 追踪服务
- Jupyter Lab
- 训练调度器
