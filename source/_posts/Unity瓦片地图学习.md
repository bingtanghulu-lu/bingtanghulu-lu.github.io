---
title: Unity瓦片地图学习
date: 2026-06-24 09:03:11
tags:
---

## 如何创建瓦片地图

1. 首先创建一个瓦片调色板（Tile Palette）

{% asset_img Creating Tile Palette_1.png 创建瓦片调色板 %}

创建完成后可以在 Unity 界面中看到出现了 Tile Palette 的窗口，并且可以与其它窗口合并。

{% asset_img Creating Tile Palette_2.png Tile Palette 窗口 %}

并且注意到，在创建调色板的时候，有四种类型可供选择：

- **Rectangular**（矩形瓦片调色板）

{% asset_img Rectangular.png 矩形瓦片调色板 %}

- **Isometric**（等距 / 斜 45° 菱形瓦片调色板）

{% asset_img Isometric.png 等距瓦片调色板 %}

> 这里可以看到瓦片资源和网格的对应关系不是很好，后面会有解决办法。

- **Hexagonal Pointed-Top**（尖顶六边形调色板）

{% asset_img Hexagonal Pointed-Top.png 尖顶六边形调色板 %}

- **Hexagonal Flat-Top**（平顶六边形调色板）

{% asset_img Hexagon Flat-Top Palette.png 平顶六边形调色板 %}

---

## 关于调色板界面

调色板最上方有一排图标，从左往右依次为：

1. **选择工具（Select，三角箭头图标）**
   功能：选中场景里已经绘制好的瓦片，可移动、删除、修改瓦片，不绘制新瓦片。

2. **移动视口工具（Move View，四向箭头）**
   功能：平移 Scene 窗口画布，按住鼠标拖拽就能拖动地图视角，和编辑器手型工具作用一致。

3. **画笔工具（Paint Brush，毛笔图标）**
   功能：基础绘制，单点 / 连续刷瓦片，最常用绘图模式。

4. **矩形填充工具（Rectangle，方块框）**
   功能：框选一块区域，一次性铺满瓦片，快速绘制大片地面、墙壁。

5. **取色吸管工具（Picker，吸管）**
   功能：点击场景里任意瓦片，把它拾取为当前画笔要绘制的瓦片，快速切换素材。

6. **橡皮擦工具（Eraser，橡皮）**
   功能：擦除场景中已绘制的瓦片，支持单点 / 大面积擦除。

7. **填充桶工具（Fill Bucket，油漆桶）**
   功能：区域洪水填充，连通的同一片空白区域一次性铺满瓦片，适合大面积地形填充。

{% asset_img PalettePanel.png 调色板面板 %}

往下一行的下拉框，可以在场景中有多个 Tilemap 组件时，选择需要编辑的 Tilemap，这样就可以为不同的组件设置不同的层，来实现层级间的前后关系。

{% asset_img SelectTilemap.png 选择 Tilemap %}

再往下：

- 左侧下拉菜单可以选择已有调色板
- 右侧 4 个图标从左往右依次为：

1. **Brush（画笔图标）** — 标准手绘笔刷，单点、连续涂抹绘制瓦片，基础绘图工具。
2. **Rectangle Brush（网格#图标）** — 框选矩形区域批量填充瓦片，快速绘制大片规整地形。
3. **Random Brush（球形随机图标）** — 随机选用调色板内瓦片绘制，避免画面重复，制作自然地形。
4. **Editor Settings（齿轮画笔图标）** — 画笔高级设置，调整笔刷尺寸、随机概率、动画瓦片参数等。

{% asset_img PalettePanel_2.png 调色板面板 - 画笔设置 %}

当有了调色板后，在 Hierarchy 窗口创建一个对应类型的 Tilemap 对象，再把调色板对应类型的美术资源拖入面板，就可以添加资源到场景了。

{% asset_img CreatingTilemap.png 创建 Tilemap %}

下面用 **Rectangular** 类型做讲解。

比如要把资源添加到 Scene 窗口，就可以先用取色吸管工具吸取想要的瓦片，再在 Scene 窗口画就行了。

{% asset_img DrawingTilemap.png 绘制 Tilemap %}

然后再选择对应的 Tilemap 组件，添加 **Tilemap Collider 2D**，就可以创建一个矩形瓦片地图了。如果需要优化性能，可以将 Tilemap Collider 勾选 **Used By Composite**，并且添加 **Composite Collider 2D** 将碰撞盒整合。

{% asset_img tips_1.png Tilemap Collider 设置 %}

---

## 关于等轴瓦片

最开始提到等轴瓦片资源与网格不对应的问题，可以先选中对应资源：

{% asset_img SelectTileArt.png 选择瓦片资源 %}

找到对应资源，使用精灵图片编辑器进行中心点的编辑：

{% asset_img SpriteEditor.png 精灵图片编辑器 %}

这里将一个瓦片资源中心点设置为图片上方中心，一个为下方中心：

{% asset_img CenterPoint_1.png 中心点设置 - 上方中心 %}
{% asset_img CenterPoint_2.png 中心点设置 - 下方中心 %}

不过在使用的时候，会发现有一些渲染问题，这个时候需要打开 Project Settings 做一些设置：

{% asset_img tips_1.png Project Settings 渲染设置 %}

然后就可以将调色板的资源加入到场景中啦，并且两个资源之间还能表现出高低差：

{% asset_img over.png 最终效果 %}
