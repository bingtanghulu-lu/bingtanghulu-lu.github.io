---
title: Unity图集缓存工具:嵌套Dictionary的实践与C#引用类型理解
date: 2026-06-12 17:44:52
tags:
---

## 为何要设计这个工具类

　　起因是在学习SpriteRenderer精灵渲染器时留下的一道习题，因为想要在代码里面调用图集里的某个关键帧
{% asset_img 1.png 关键帧截图 %}
需要先遍历Texture2D类型图片(如果Sprite Mode = Multiple(多图模式))下分割出的若干sprite类型图片，将其储存在一个sprite数组里，再单独通过索引取出使用

```C#
//动态的加载图集中的图
Sprite[] sprs = Resources.LoadAll<Sprite>("RobotBoyIdleSprite");
sr.sprite = sprs[10];

print(sprs[10].name);
```

所以就让设计一个简便的方法进行调用

　　因为自己写的方法过于简单(直接传大图名字加索引进行封装)，所以就不讨论了。教程做法是用字典套字典的方法<string,Dictionary<string,Sprite>>，外层是用大图名字做key，然后内层当然就是小图名字做key，然后sprite做value,这样子就可以指定传入大图名字，然后再去检索是否有想要的小图，比我的方法好在指定更具体，更便于查看字典是否储存有指定的sprite。

```C#
        //首先判断传入的大图是否已经被加载过了
        //如果有，则直接加载
        if(dic.ContainsKey(multipleName))
        {
            //判断此大图里是否有这个sprite小图
            if(dic[multipleName].ContainsKey(spriteName))
            {
                return dic[multipleName][spriteName];
            }
        }
        else
        {
            //定义一个储存sprite的字典
            Dictionary<string , Sprite> dicTmp = new Dictionary<string, Sprite>();
            //读取指定的大图里面的sprite小图，存入sprite数组
            Sprite[] spr = Resources.LoadAll<Sprite>(multipleName);
            //将读取的sprite加载入字典
            for(int i =0;i<spr.Length;i++)
            {
                dicTmp.Add(spr[i].name,spr[i]);
            }
            //判断最开始传入的spriteName是不是真的存在，有则返回，反之null
            if(dicTmp.ContainsKey(spriteName))
            {
                return dicTmp[spriteName];
            }
        }
        return null;
```

方法设计逻辑真的很精妙，环环相扣。具体逻辑注释已经写的很清楚了。
　　通过这个嵌套字典的使用可以学到，在之后遇到类似的，一个父类包裹子类，也能这样子储存和写工具类快速查询，并且对数据类型也有了更深的认识，因为一开始很诧异为什么LoadAll不会读到大图，但实际上是读到了，只不过因为泛型指定了Sprite类型，所以忽略了。
　　其次，这段代码还有一个清除方法

```C#
    public void ClearInfo()
    {
        dic.Clear();

        Resources.UnloadUnusedAssets();
    }
```

　　起初我不理解为什么还要写Resources.UnloadUnusedAssets();因为我当时认为，加载出来的资源不就是要用的吗，不然我加载来干啥，怎么会存在没有被引用的资源。但实际上

- dicTmp和spr 是局部变量，当函数调用结束后，就被销毁了，dicTmp和spr 中的 Sprite 全部变成无引用的资源。因为Resources.LoadAll 把 Sprite 加载到内存中，spr 数组和 dicTmp 字典都只是持有这些 Sprite 的引用。new Dictionary<string, Sprite>()相当于在堆上实际搭建了一个 Dictionary 数据结构（分配内存 + 初始化内部状态）。变量 dicTmp 拿到的是这个文件柜的坐标（地址/地图），而不是文件柜本身。之后通过这张地图去找文件柜，往里面存取数据。Add就相当于拿着地图 dicTmp 找到仓库里那个文件柜，打开抽屉，把 Sprite 的名字写成标签贴上去，然后把对应存放Sprite的仓库区域的那张地图放进抽屉里。整个过程是：地图 → 文件柜 → 存入内容。
- 所以对于引用类型来说，每次如果有另外一个对象引用比如同一个sprit，就不需要复制一份一样的sprite，而是直接把sprite的地址拷贝一份，就可以找到同一份数据了。反之，像值类型这种，没有"地图"这一层间接，它就是数据本身。放栈上就是栈上的数据，放在堆上（作为类字段）就是堆上的数据，每次引用都是重写拷贝一份
- 这也就是为什么指向同一个数据，改变值类型，另一个不受影响，而引用类型会。

## 感受

　　第一篇正式的心得博客，不太熟练，口水话很多，关键还是在于对知识点的提取不够精炼吧。
