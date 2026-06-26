---
title: openslide封装成自己的app(PyQt)
date: 2024-03-18 10:00:00
updated: 2024-03-18 10:00:00
tags:
- openslide
- PyQt
- Python
- WSI
categories:
- 技术记录
---

> 本文原发布于 CSDN：<https://blog.csdn.net/Xi1yang/article/details/136816122>

OpenSlide 是一个 C 库，提供了一个简单的阅读界面，用于读取全幻灯片图像。

全幻灯片图像也称为虚拟幻灯片，通常是高分辨率数字病理学中使用的图像。这些图像可以占用数十千兆字节，未压缩时甚至更多，因此无法使用标准工具轻松读取。

专为可以舒适地未压缩到 RAM 中的多分辨率图像，通常无法直接处理这些图像。

安装之后，我们可以发现给出的 demo 可以直接展示切片图像。

### 一、安装依赖

首先需要安装 OpenSlide 相关依赖。

Python 环境中可以安装：

```bash
pip install openslide-python
```

如果系统中没有安装 OpenSlide 动态库，还需要先安装 OpenSlide 本体。

Windows 环境下可以下载 OpenSlide Windows binaries，然后把对应目录加入环境变量。

### 二、PyQt 界面思路

封装成自己的 app 时，可以使用 PyQt 搭建一个简单界面。

核心功能包括：

- 打开切片文件。
- 读取切片基本信息。
- 显示缩略图。
- 根据坐标读取指定区域。
- 支持缩放和拖动查看。

### 三、读取切片文件

可以使用 `openslide.OpenSlide` 打开切片：

```python
import openslide

slide = openslide.OpenSlide("demo.svs")

print(slide.dimensions)
print(slide.level_count)
print(slide.level_dimensions)
```

常用属性包括：

- `dimensions`：最高分辨率下的宽高。
- `level_count`：金字塔层级数量。
- `level_dimensions`：每一层的宽高。
- `properties`：切片元数据。

### 四、生成缩略图

可以先生成缩略图用于界面预览：

```python
thumbnail = slide.get_thumbnail((800, 800))
thumbnail.save("thumbnail.png")
```

在 PyQt 中可以把 PIL 图像转换为 Qt 图像，再显示到界面控件中。

### 五、读取指定区域

OpenSlide 可以读取指定位置的一块区域：

```python
region = slide.read_region((x, y), level, (width, height))
region = region.convert("RGB")
region.save("region.png")
```

参数说明：

- `(x, y)`：在最高分辨率层级下的坐标。
- `level`：要读取的金字塔层级。
- `(width, height)`：读取区域大小。

### 六、PyQt 封装结构

一个简单的项目结构可以是：

```text
openslide_app/
├── main.py
├── viewer.py
├── slide_reader.py
└── requirements.txt
```

其中：

- `main.py`：程序入口。
- `viewer.py`：PyQt 主窗口和交互逻辑。
- `slide_reader.py`：封装 OpenSlide 读取逻辑。
- `requirements.txt`：依赖记录。

### 七、常见问题

#### 找不到 openslide 动态库

如果报错找不到 OpenSlide 动态库，通常是系统环境变量没有配置好。

Windows 下需要把 OpenSlide 的 `bin` 目录加入 `Path`。

#### 大图显示卡顿

不要一次性读取整张 WSI 图像。

建议按当前视窗范围读取局部区域，并根据缩放比例选择合适的金字塔层级。

#### 坐标不准确

注意 `read_region` 中的坐标通常是最高分辨率层级下的坐标。显示缩略图时需要做好坐标换算。

### 小结

OpenSlide 适合读取超大病理切片图像，PyQt 适合封装桌面界面。

实际封装时，建议把图像读取逻辑和界面逻辑分开，先保证能读取切片、生成缩略图、读取局部区域，再逐步添加缩放、拖动和标注等功能。
