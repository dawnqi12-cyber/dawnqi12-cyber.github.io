---
title: toolkit从源头构建
date: 2025-10-01 01:52:12
tags:
- python
- toolkit
categories:
- 技术学习
---
### 目标

&nbsp;&nbsp;&nbsp;&nbsp;我计划构建一个集成了多种实用功能的个人工具库用于解决日常问题，并且系统性的提升工程能力，以下危一套从基础框架到持续迭代的完整思路。


项目结构​​：采用标准的Python包结构，这样未来打包发布会非常方便。

    my_toolkit/ # 项目根目录
    ├── src/ # 源代码目录
    │ └── my_toolkit
    │ ├── init.py 
    │ ├── image_utils.py 
    │ ├── audio_utils.py 
    │ └── cli.py # 命令行入口
    ├── tests/ 
    │ ├── init.py 
    │ ├── test_image_utils.py 
    │ └── test_audio_utils.py #
    ├── docs/
    ├── README.md
    ├── requirements.txt 
    └── setup.py 

Python版本选择：python3.8.0

### 第一步：从核心功能开始
1. 初始化项目环境
        # 进入项目目录
        cd my_toolkit

        # 创建虚拟环境
        python -m venv venv

        # 激活虚拟环境
        venv\Scripts\activate

        # 安装基础依赖
        pip install pillow pygame  # 图片和音频处理核心库
2. 实现第一个使用功能

在 src/my_toolkit/image_utils.py中：
```python
from PIL import Image
import os

def resize_images(input_folder, output_folder, size=(800, 600)):
    """
    批量调整图片大小
    """
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)
    
    for filename in os.listdir(input_folder):
        if filename.lower().endswith(('.png', '.jpg', '.jpeg')):
            input_path = os.path.join(input_folder, filename)
            output_path = os.path.join(output_folder, filename)
            
            with Image.open(input_path) as img:
                img_resized = img.resize(size, Image.Resampling.LANCZOS)
                img_resized.save(output_path)
            print(f"已处理: {filename}")

if __name__ == "__main__":
    # 测试代码
    resize_images("./input", "./output")
```
3. ​​创建命令行接口​

在 src/my_toolkit/cli.py中：
```python
import argparse
from . import image_utils

def main():
    parser = argparse.ArgumentParser(description="我的个人工具库")
    subparsers = parser.add_subparsers(dest='command', help='可用命令')
    
    # 图片处理命令
    img_parser = subparsers.add_parser('resize', help='调整图片大小')
    img_parser.add_argument('--input', required=True, help='输入文件夹')
    img_parser.add_argument('--output', required=True, help='输出文件夹')
    img_parser.add_argument('--width', type=int, default=800, help='宽度')
    img_parser.add_argument('--height', type=int, default=600, help='高度')
    
    args = parser.parse_args()
    
    if args.command == 'resize':
        image_utils.resize_images(args.input, args.output, (args.width, args.height))
        print("图片处理完成！")

if __name__ == "__main__":
    main()
```
4. 配置打包文件

在 setup.py中：
```python
from setuptools import setup, find_packages

setup(
    name="my-toolkit",
    version="0.1.0",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    install_requires=[
        "Pillow>=8.0.0",
        "pydub>=0.25.0",
    ],
    entry_points={
        'console_scripts': [
            'mytool=my_toolkit.cli:main',
        ],
    },
    author="Your Name",
    description="个人实用工具集合",
) 
```

📋 第一步的具体行动清单
    ✅ 创建虚拟环境​​ → python -m venv venv
    
    ​​✅ 激活虚拟环境​​ → venv\Scripts\activate
    ​​✅ 安装基础依赖​​ → pip install pillow pydub
    ​​🖼️ 实现图片批量 resize 功能​​ → 编辑 image_utils.py
    ​​⌨️ 创建命令行接口​​ → 编辑 cli.py
    ​​📦 配置打包文件​​ → 完善 setup.py和 requirements.txt
    ​​🧪 测试功能​​ → python -m my_toolkit.cli resize --input ./test_input --output ./test_output

![这是开发流程](开发流程.png)

~~谁来告诉我这个图片大小怎么调整最方便啊啊啊~~
