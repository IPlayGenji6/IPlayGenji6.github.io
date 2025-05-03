---
title: UE5 中使用 Git
---

## 使用 Git 管理 UE5 项目

在 Unreal Engine 5 (UE5) 中使用 Git 管理项目是一个常见的版本控制方法。以下是一些关键流程和注意事项：

### 开启大文件支持
使用终端或命令行运行以下命令完成 LFS 初始化：
   ```bash
   git lfs install
   ```

### 配置 `.gitignore`
示例
```plaintext
# Ignore all files by default, but scan all directories.
*
!*/

# Do not ignore git files in the root of the repo.
!/.git*

# Do not ignore current project's `.uproject`.
!/*.uproject

# Do not ignore source, config and plugins dirs.
!/Source/**
!/Config/**
!/Plugins/**

# Only allow .uasset and .umap files from /Content dir.
# They're tracked by git-lfs, don't forget to track other
# files if adding them here.
!/Content/**/*.uasset
!/Content/**/*.umap

# Allow any files from /RawContent dir.
# Any file in /RawContent dir will be managed by git lfs.
!/RawContent/**/*

# OS/platform generated files.

# Windows
ehthumbs.db
Thumbs.db

# Mac OS X
.DS_Store
.DS_Store?
.AppleDouble
.LSOverride
._*

# Linux
*~
.directory

# vim
[._]*.s[a-w][a-z]
[._]s[a-w][a-z]
*.un~
Session.vim
.netrwhist

# Visual Studio
.vs
```

### 配置 `.gitattributes`
```plaintext
# Unreal Engine file types.
*.uasset filter=lfs diff=lfs merge=lfs -text
*.umap filter=lfs diff=lfs merge=lfs -text

# Raw Content file types.
*.3ds filter=lfs diff=lfs merge=lfs -text
*.bmp filter=lfs diff=lfs merge=lfs -text
*.exr filter=lfs diff=lfs merge=lfs -text
*.fbx filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.mov filter=lfs diff=lfs merge=lfs -text
*.mp3 filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
*.obj filter=lfs diff=lfs merge=lfs -text
*.ogg filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.tga filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.xcf filter=lfs diff=lfs merge=lfs -text

# Anything in `/RawContent` dir.
/RawContent/**/* filter=lfs diff=lfs merge=lfs -text
```

