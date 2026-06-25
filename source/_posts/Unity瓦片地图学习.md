---
title: Unity瓦片地图学习
date: 2026-06-24 09:03:11
tags: 
  - Unity
  - Tilemap
categories:
  - Unity学习
---

## 如何创建瓦片地图

1. 首先创建一个瓦片调色板（Tile Palette）

{% asset_img Creating-Tile-Palette_1.png 创建瓦片调色板 %}

创建完成后可以在 Unity 界面中看到出现了 Tile Palette 的窗口，并且可以与其它窗口合并。

{% asset_img Creating-Tile-Palette_2.png Tile Palette 窗口 %}

在创建调色板时，有四种类型可供选择：

| 类型 | 说明 |
| ---- | ---- |
| **Rectangular** | 矩形瓦片调色板 |
| **Isometric** | 等距 / 斜 45° 菱形瓦片调色板 |
| **Hexagonal Pointed-Top** | 尖顶六边形调色板 |
| **Hexagonal Flat-Top** | 平顶六边形调色板 |

{% asset_img Rectangular.png 矩形瓦片调色板 %}

{% asset_img Isometric.png 等距瓦片调色板 %}

> 这里可以看到 Isometric 类型瓦片资源和网格的对应关系不是很好，后面会有解决办法。

{% asset_img Hexagonal-Pointed-Top.png 尖顶六边形调色板 %}

{% asset_img Hexagon-Flat-Top-Palette.png 平顶六边形调色板 %}

---

## 关于调色板界面

调色板最上方有一排图标，从左往右依次为：

| 图标 | 名称 | 功能 |
| ---- | ---- | ---- |
| 三角箭头 | **选择工具（Select）** | 选中场景中已绘制的瓦片，可移动、删除、修改瓦片，不绘制新瓦片 |
| 四向箭头 | **移动视口工具（Move View）** | 平移 Scene 窗口画布，按住鼠标拖拽即可拖动地图视角，和编辑器手型工具作用一致 |
| 毛笔 | **画笔工具（Paint Brush）** | 基础绘制，单点 / 连续刷瓦片，最常用绘图模式 |
| 方块框 | **矩形填充工具（Rectangle）** | 框选一块区域，一次性铺满瓦片，快速绘制大片地面、墙壁 |
| 吸管 | **取色吸管工具（Picker）** | 点击场景中任意瓦片，将其拾取为当前画笔要绘制的瓦片，快速切换素材 |
| 橡皮 | **橡皮擦工具（Eraser）** | 擦除场景中已绘制的瓦片，支持单点 / 大面积擦除 |
| 油漆桶 | **填充桶工具（Fill Bucket）** | 区域洪水填充，连通的同一片空白区域一次性铺满瓦片，适合大面积地形填充 |

{% asset_img PalettePanel.png 调色板面板 %}

往下一行的下拉框，可以在场景中有多个 Tilemap 组件时，选择需要编辑的 Tilemap，为不同组件设置不同层来实现层级间的前后关系。

{% asset_img SelectTilemap.png 选择 Tilemap %}

再往下：

- 左侧下拉菜单可以选择已有调色板
- 右侧 4 个图标从左往右依次为：

| 图标 | 名称 | 功能 |
| ---- | ---- | ---- |
| 画笔 | **Brush** | 标准手绘笔刷，单点、连续涂抹绘制瓦片，基础绘图工具 |
| 网格# | **Rectangle Brush** | 框选矩形区域批量填充瓦片，快速绘制大片规整地形 |
| 球形随机 | **Random Brush** | 随机选用调色板内瓦片绘制，避免画面重复，适合制作自然地形 |
| 齿轮画笔 | **Editor Settings** | 画笔高级设置，调整笔刷尺寸、随机概率、动画瓦片参数等 |

{% asset_img PalettePanel_2.png 调色板面板 - 画笔设置 %}

当有了调色板后，在 Hierarchy 窗口创建一个对应类型的 Tilemap 对象，再把调色板对应类型的美术资源拖入面板，即可添加资源到场景中。

{% asset_img CreatingTilemap.png 创建 Tilemap %}

---

### 实操：Rectangular 类型

下面用 **Rectangular** 类型做讲解。

比如要把资源添加到 Scene 窗口，先用取色吸管工具吸取想要的瓦片，再在 Scene 窗口绘制即可。

{% asset_img DrawingTilemap.png 绘制 Tilemap %}

选择对应的 Tilemap 组件，添加 **Tilemap Collider 2D**，即可创建矩形瓦片地图。

> 如需优化性能，可将 Tilemap Collider 勾选 **Used By Composite**，并添加 **Composite Collider 2D** 将碰撞盒整合。

{% asset_img tips_1.png Tilemap Collider 设置 %}

---

## 关于等轴瓦片

最开始提到等轴瓦片资源与网格不对应的问题，可以先选中对应资源：

{% asset_img SelectTileArt.png 选择瓦片资源 %}

找到对应资源，使用精灵图片编辑器（Sprite Editor）进行中心点的编辑：

{% asset_img SpriteEditor.png 精灵图片编辑器 %}

将一个瓦片资源中心点设置为图片上方中心，另一个为下方中心：

{% asset_img CenterPoint_1.png 中心点设置 - 上方中心 %}
{% asset_img CenterPoint_2.png 中心点设置 - 下方中心 %}

不过在使用的过程中可能会遇到渲染问题，此时需要打开 **Project Settings** 做一些设置：

{% asset_img tips_1.png Project Settings 渲染设置 %}

然后就可以将调色板的资源加入到场景中啦，两个资源之间还能表现出高低差：

{% asset_img over.png 最终效果 %}

---

## 使用等轴瓦片制作一个简单的伪 3D 场景

首先，将上一步内容完善，搭建一个简单的沙盒，并且拖入角色 Sprite 图片：

{% asset_img 01.png 搭建沙盒并拖入角色 %}

可以看到场景实现了遮挡效果，但是角色与地图之间还存在渲染问题。这时我们可以选择对应的 Tilemap 对象，可以看到上面挂载的 Tilemap Renderer 组件上有 **Order in Layer** 属性，假设地图为 0 层，我们可以将角色改为 1 层，就可以实现角色在地图前显示了：

{% asset_img 02.png 设置 Order in Layer %}

但是当我们把角色移动至下方围墙时可以看到，并没有如我们想象中那样呈现一个正常的遮挡效果，因为此时，不管是"地面"还是"墙体"，本质上都是由同一个 Tilemap Renderer 渲染的瓦片对象，所以在同一层。但是一个 Grid（网格容器）可以同时管理多个 Tilemap 对象，也就能有多个渲染器并多层渲染。所以我们可以在 Grid 下方再次创建一个 Tilemap，专门管理要对角色进行遮盖的墙体部分，将其层级大于角色：

{% asset_img 03.png 创建独立墙体 Tilemap %}

使用同样的方法，我们还可以制作场景中类似植物的物品：

{% asset_img 04.png 制作植物物品 %}

但需要注意几点：

- 记得更改对应角色和物品的中心点，尽量改为底部，并且排序方式改为 Pivot
- 这种方式只能够在一个格子放置一个物体（如果需要在一个格子放置多个物体可以将 Sprite 图片直接拖入到场景，更改为对应层级）

{% asset_img 05.png 中心点与排序设置 %}
{% asset_img 06.gif 动态效果展示 %}

搭建完成地形后，就可以为地图添加 Tilemap Collider 来实现碰撞效果（记得给角色也做对应设置：碰撞盒、刚体，取消重力）。但是如果直接添加，可以看到碰撞盒铺满了整个地图：

{% asset_img 07.png 碰撞盒铺满地图 %}

但其实对应等轴游戏，地面与角色之间是不需要碰撞的，否则将被一大堆碰撞盒卡死、无法动弹，所以需要把碰撞类型——地面改为 **None**，墙体改为 **Grid**。

瓦片的碰撞类型有以下三种：

| 碰撞类型 | 效果 | 适用场景 |
| -------- | ---- | -------- |
| **None** | 完全不生成碰撞体，角色、子弹可直接穿过，性能最优 | 草地、道路、水面、远景装饰、纯背景地砖 |
| **Sprite** | 碰撞形状匹配精灵的自定义物理轮廓，可贴合不规则图形 | 不规则障碍物、悬崖、斜坡、道具石块 |
| **Grid** | 碰撞体为标准单元格矩形，等轴地图自动适配菱形轮廓，配合 Composite Collider 可合并优化 | 方正墙体、平台、实心地板、连续大片地面 |

> **注意**：使用 Sprite 类型时，精灵需要提前在 Sprite Editor 里编辑物理轮廓（Custom Physics Shape），否则自动生成的轮廓会有误差；碰撞形状不规则，物理计算开销略大于 Grid。

{% asset_img 08.png 地面碰撞设为 None %}
{% asset_img 09.png 墙体碰撞设为 Grid %}

然后为角色编写一个简单的移动脚本，就完成了一个简单的等轴游戏场景搭建：

```csharp
public class PlayerObj : MonoBehaviour
{
    private float x;
    private float y;
    public float Speed;

    void Update()
    {
        x = Input.GetAxis("Horizontal");
        y = Input.GetAxis("Vertical");
        transform.Translate(Vector2.right * Speed * Time.deltaTime * x);
        transform.Translate(Vector2.up * Speed * Time.deltaTime * y);
    }
}
```

{% asset_img 10.gif 角色移动效果 %}

不过此时玩家的头部还是会和上方地图产生碰撞，无法贴合墙壁。解决方法也很简单，去除上方墙壁的碰撞，在外围适当位置添加隐形的碰撞即可。
