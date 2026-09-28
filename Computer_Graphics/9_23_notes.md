# 光栅显示与位面（Bit Plane）

## 1. 光栅式显示（Raster Display）

### 核心思想

将图像表示为二维像素矩阵：

```
Pixel Pixel Pixel
Pixel Pixel Pixel
Pixel Pixel Pixel
```

显示时按照固定顺序逐行扫描：

```
第1行 → 第2行 → ... → 第N行
```

现代 LCD、OLED 等显示器虽然不再使用电子束，但仍然采用：

```
像素矩阵 + 刷新显示
```

因此属于光栅显示。

---

# 2. 帧缓冲器（Framebuffer）

## 定义

Framebuffer 是保存一帧图像像素数据的存储区域。

例如：

```
Pixel(x,y) = RGB颜色值
```

GPU渲染产生像素后写入：

```
GPU
 |
 ↓
Framebuffer
 |
 ↓
Display Controller
 |
 ↓
显示器
```

显示器读取Framebuffer中的数据并刷新屏幕。

---

# 3. 位（Bit）

计算机中最小数据单位：

```
0 或 1
```

例如：

一个8 bit像素：

```
10110101

bit7 bit6 ... bit0
```

---

# 4. 位面（Bit Plane）

## 定义

位面是：

> 将所有像素中相同位置的bit提取出来形成的二维平面。

例如8bit灰度图：

原始：

```
像素A: 10110101
像素B: 01001100
```

拆分：

```
Bit7平面：

1 0 ...


Bit6平面：

0 1 ...


...


Bit0平面：
```

每个位面就是一张黑白图。

---

# 5. 为什么需要位面？

早期显卡显存有限，采用位面组织：

```
Plane0
Plane1
Plane2
Plane3
```

每个位面保存一个bit。

优点：

- 硬件实现简单
- 方便修改某一位信息
- 适合早期图像处理

缺点：

读取一个完整像素需要访问多个位面。

---

# 6. 位面式 vs 像素式存储

## 位面式（Planar）

按bit存：

```
Plane3
Plane2
Plane1
Plane0
```

特点：

```
同一个bit集中存储
```

---

## 像素式（Packed Pixel）

现代GPU常用：

例如RGBA：

```
Pixel0:
RGBA

Pixel1:
RGBA
```

特点：

```
一个像素的数据连续存储
```

方便GPU进行：

- 纹理采样
- 光照计算
- 3D渲染

---

# 7. 现代图形系统关系

现代GPU流程：

```
3D模型

↓

光栅化 Rasterization

↓

Framebuffer

↓

显示器刷新

↓

屏幕像素
```

现代显示仍是光栅显示，但Framebuffer通常采用像素式存储，而不是早期的位面式存储。

