---
title: git学习记录
date: 2025-10-01 06:12:15
tags:
- 问题记录
category:
- 技术记录
---


### 🚀 上传代码的正确流程
1. 添加需要跟踪的文件
    
    #添加所有修改和新增的代码文件（排除不需要上传的）
    ```
    git add requirements.txt setup.py src/my_toolkit/cli.py src/my_toolkit/image_utils.py pyproject.toml readme.md
    ```
2. ​​创建.gitignore文件​​（如果还没有）

    #创建.gitignore文件并添加以下内容
    ```
    echo "__pycache__/" > .gitignore
    echo "*.egg-info/" >> .gitignore
    echo "input_images/" >> .gitignore
    echo "output_images/" >> .gitignore
    ```

3. ​​检查忽略规则​

    #检查哪些文件会被忽略
    ```
    git status --ignored
    ```
4. ​​提交更改
    ```​
    git commit -m 
    ```
5. ​​推送到远程仓库​
    ```
    git push origin main
    ```
### 📝 关键注意事项
1. 不要上传的目录/文件​​：
    - _pycache_/（Python缓存）
    - *.egg-info/（构建生成文件）
    - input_images/（测试输入）
    - output_images/（测试输出）

2. ​​必须上传的重要文件​​：
    - pyproject.toml（新添加的构建配置）
    - readme.md（项目文档）
    - src/下的所有.py文件（核心代码）
3. 如果遇到冲突​​：
    ```
    #先拉取最新代码
    git pull origin main
    #解决冲突后重新提交
    git add .
    git commit -m "解决合并冲突"
    git push origin main
    ```
### 💡 建议的后续操作
1. ​​创建版本标签
    ```
    git tag v0.1.0
    git push origin v0.1.0
    ```

2. ​​检查远程仓库状态​​
    ```
    git remote show origin
    ```
3. 验证推送结果
    ```
    git log --oneline -n 5  # 查看最近提交
    git ls-remote origin    # 检查远程引用
    ```