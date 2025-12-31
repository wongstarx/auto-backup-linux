# 故障排除指南

## ModuleNotFoundError: No module named 'auto_backup'

### 问题原因

这个错误通常是因为：
1. GitHub 仓库中的代码还没有更新（文件还在根目录，不在 `auto_backup/` 目录）
2. pipx 使用了缓存的旧版本
3. 包结构不正确

### 解决方案

#### 方案一：清除缓存并强制重新安装（推荐）

```bash
# 1. 完全卸载
pipx uninstall auto-backup-linux

# 2. 清除 pipx 缓存
pipx cache clear

# 3. 强制重新安装（不使用缓存）
pipx install --force git+https://github.com/wongstarx/auto-backup-linux.git

# 4. 测试
auto-backup
```

#### 方案二：从特定分支/提交安装

如果主分支还没有更新，可以尝试：

```bash
# 卸载旧版本
pipx uninstall auto-backup-linux

# 从特定分支安装（如果有修复分支）
pipx install git+https://github.com/wongstarx/auto-backup-linux.git@fix-package-structure

# 或从特定提交安装
pipx install git+https://github.com/wongstarx/auto-backup-linux.git@<commit-hash>
```

#### 方案三：本地安装测试

如果 GitHub 还没有更新，可以临时从本地安装：

```bash
# 在本地仓库目录
cd /path/to/auto-backup-linux

# 使用 pipx 从本地安装
pipx install -e .

# 或使用 pip 在虚拟环境中安装
python3 -m venv venv
source venv/bin/activate
pip install -e .
```

#### 方案四：检查已安装的包结构

```bash
# 查看 pipx 安装的包位置
pipx list --verbose

# 检查包的实际结构
ls -la ~/.local/pipx/venvs/auto-backup-linux/lib/python*/site-packages/

# 应该看到 auto_backup 目录
ls ~/.local/pipx/venvs/auto-backup-linux/lib/python*/site-packages/auto_backup/
```

### 验证修复

安装后，验证包结构：

```bash
# 方法1：检查 Python 能否导入
python3 -c "import auto_backup; print(auto_backup.__version__)"

# 方法2：检查入口点
which auto-backup

# 方法3：查看包内容
python3 -c "import auto_backup; print(dir(auto_backup))"

# 方法4：直接运行
auto-backup
```

### 如果问题仍然存在

1. **检查 GitHub 仓库是否已更新**：
   ```bash
   # 查看仓库结构
   git ls-tree -r HEAD --name-only | grep -E "(auto_backup|cli\.py)"
   ```

2. **手动检查安装的包**：
   ```bash
   # 找到安装位置
   pipx list --verbose | grep auto-backup
   
   # 进入虚拟环境
   ~/.local/pipx/venvs/auto-backup-linux/bin/python -c "import sys; print(sys.path)"
   ```

3. **重新构建包**：
   ```bash
   # 在本地仓库
   python3 -m build
   
   # 检查构建的包
   ls -la dist/
   ```

### 临时解决方案

如果急需使用，可以创建一个包装脚本：

```bash
# 创建 ~/bin/auto-backup-wrapper.sh
cat > ~/bin/auto-backup-wrapper.sh << 'EOF'
#!/bin/bash
cd /path/to/auto-backup-linux
source venv/bin/activate  # 如果有虚拟环境
python3 -m auto_backup.cli "$@"
EOF

chmod +x ~/bin/auto-backup-wrapper.sh
```

### 确保 GitHub 仓库已更新

在推送修复到 GitHub 之前，确保：

1. ✅ 文件已移动到 `auto_backup/` 目录
2. ✅ `setup.py` 中的入口点正确：`auto_backup.cli:main`
3. ✅ `pyproject.toml` 中的入口点正确：`auto_backup.cli:main`
4. ✅ 版本号已更新（避免缓存问题）

然后提交并推送：

```bash
git add auto_backup/ setup.py pyproject.toml auto_backup/__init__.py
git commit -m "Fix: Move files to auto_backup package directory (v1.0.1)"
git push origin main
```

### 检查清单

- [ ] GitHub 仓库中的文件在 `auto_backup/` 目录下
- [ ] `setup.py` 使用 `find_packages()` 能找到 `auto_backup`
- [ ] 入口点配置为 `auto_backup.cli:main`
- [ ] 版本号已更新（避免缓存）
- [ ] 已清除 pipx 缓存
- [ ] 使用 `--force` 重新安装

