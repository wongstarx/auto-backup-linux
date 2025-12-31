# 发布到PyPI指南

## 准备工作

1. 注册PyPI账号：https://pypi.org/account/register/
2. 安装必要的工具：
```bash
pip install build twine
```

## 构建包

```bash
# 在项目根目录执行
python -m build
```

这会在 `dist/` 目录下生成分发包文件。

## 上传到PyPI

### 测试上传（TestPyPI）

```bash
# 上传到测试服务器
twine upload --repository testpypi dist/*

# 测试安装
pip install --index-url https://test.pypi.org/simple/ auto-backup-linux
```

### 正式发布

```bash
# 上传到PyPI
twine upload dist/*
```

## 更新版本

1. 修改 `setup.py` 中的 `version` 字段
2. 更新 `README.md` 中的更新日志
3. 重新构建和上传

## 注意事项

- 确保所有文件都已提交到Git
- 检查 `setup.py` 中的元数据是否正确
- 确保 `README.md` 格式正确
- 测试安装和运行是否正常

