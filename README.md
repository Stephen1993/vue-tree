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
<img src = "https://github.com/Stephen1993/vue-tree/blob/master/img/g6ift-is84s.gif" display = "flex" width = "300px" height = "500px" >

示例：
```
<template>
  <div>
    <launch-tree :list='list' :options='options'></launch-tree>
  </div>
</template>

<script>
import launchTree from '../components/launch_tree.vue'

const COMPONENT_NAME = 'Login'
let list = [
  {
    name: '一级目录', // 目录名字
    isOpen: true, // 是否初始展开目录
    hightLight: true, // 是否初始高亮
    className: undefined, // 添加自定义样式
    ...{}, // 其他用户额外参数
    childs: [ // 二级目录
      {
        name: '二级目录1',
        isOpen: false
      },
      {
        name: '二级目录2'
      },
      {
        name: '二级目录3'
      }
    ]
  },
  {
    name: '二级目录', // 目录名字
    isOpen: true, // 是否初始展开目录
    hightLight: true, // 是否初始高亮
    className: undefined, // 添加自定义样式
    ...{}, // 其他用户额外参数
    childs: [ // 二级目录
      {
        name: '二级目录1',
        isOpen: false
      },
      {
        name: '二级目录2'
      },
      {
        name: '二级目录3'
      }
    ]
  }
]

let options = {
  callback: undefined // 自定义点击事件，callback(node)
}

export default {
  name: COMPONENT_NAME,
  data () {
    return {
      list: list,
      options: options
    }
  },
  components: {
    launchTree
  }
}

</script>
```
