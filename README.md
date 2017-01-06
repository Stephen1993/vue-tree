# vue-tree
vue 编写的树形菜单，小巧实用，支持vue1.0，vue2.0  
```
v1.0 功能:  
1.支持多级树目录  
2.支持高亮点击的节点  
3.支持展开点击节点  
4.支持点击收缩节点时收缩所有子目录  
5.支持自定义回调函数，点击节点时回调，参数为节点信息  
```

用法：`<launch-tree :list='list' :options='options'></launch-tree>`

    list = [
        {
            name: '一级目录', // 目录名字
            isOpen: true, // 是否初始展开目录
            hightLight: true, // 是否初始高亮
            className: undefined, // 添加自定义样式
            childs: [], // 二级目录
            ...{} // 其他用户额外参数 
        }
    ]
    options = {
        callback: undefined // 自定义点击事件，callback(node)
    }

图示:
![图示](http://github-image.oss-cn-hangzhou.aliyuncs.com/tmpdir--17_1_6_21_43_29.gif)
