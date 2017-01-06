<style lang="scss">
.tree-box {
  padding: 10px 25px;
  height: 100%;
  overflow: hidden;
  overflow-y: auto;
  min-height: 400px;
  background-color: #dddddd;

  .ellipsis {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }

  p {
    padding: 0;
    margin: 0;
  }

  .tree-ul {
    padding-left: 20px !important;
  }

  .tree-ul:first-child {
    padding-left: 0 !important
  }

  .folder, .node {
    cursor: pointer;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }

  .folder-box {
    cursor: pointer;
  }

  .folder-logo {
    display: inline-block;
    width: 12px;
    height: 11px;
    line-height: 7px;
    font-weight: 400;
    border: 1px solid #8e7878;
    color: #000;
    background: rgba(91, 192, 222, 0.1);
    text-align: center;
    float: left;
    margin-left: -15px;
    margin-top: 6px;
    margin-right: 5px;
  }

  .folder-true {
    color: #2494f2;
  }

  .node-true {
    color: #2494f2;
  }
}
</style>

<template>
  <div id = 'tree-box' class = 'tree-box' v-if='list'>
    <tree_item :list='list' :pnode.once='{root: true, isOpen: true}'></tree_item>
  </div>
</template>

<script type="text/javascript">
let DEFAULT_NODE = [
  {
    name: '一级目录', // 目录名字
    isOpen: true, // 是否初始展开目录
    hightLight: true, // 是否初始高亮
    className: undefined, // 添加自定义样式
    childs: [], // 二级目录
    ...{} // 其他用户额外参数
  }
]

let DEFAULT_OPTIONS = {
  callback: undefined // 自定义点击事件，callback(node)
}

let pageParams = {
  node: {default: true},
  folder: {default: true}
}

let vm = null

export default {
  props: {
    list: {
      type: Array,
      required: false,
      default: function () {
        return DEFAULT_NODE
      }
    },

    options: {
      type: Object,
      default: function () {
        return DEFAULT_OPTIONS
      }
    }
  },

  methods: {
    initTree() {
      let tempList = JSON.parse(JSON.stringify(this.list))
      let loadDatas = function(datas) {
        datas.forEach((item) => {
          item.isOpen = item.isOpen || false
          item.hightLight = item.hightLight || false
          item.className = item.className || ''
          item.childs = item.childs || []
          if(item.childs.length > 0) {
            if(item.hightLight) {
              pageParams.folder = item
            }
            loadDatas(item.childs)
          }else {
            if(item.hightLight) {
              pageParams.node = item
            }
            item.isOpen = false
          }
        })
      }
      loadDatas(tempList)
      this.list = tempList
    }
  },

  components: {
    tree_item: {
      name: 'tree_item',
      props: {
        list: {
          type: Array,
          required: false,
          default: function() {
            return DEFAULT_NODE
          }
        },

        pnode: {
          type: Object,
          default: function() {
            return {isOpen: true}
          }
        }
      },

      methods: {
        defaultClickFun(node) {
          pageParams.node.hightLight = pageParams.folder.hightLight = false
          node.isOpen = !node.isOpen
          node.hightLight = true
          if(node.childs.length > 0 && node.isOpen === false) { // 需要递归关闭子目录
            node.hightLight = false
            closeFolder(node)
          }
          if(node.childs.length > 0 && !checkChildNode(node, pageParams.folder)) { // 查询上一次点击的目录是当前目录的父目录还是兄弟目录
            pageParams.folder.isOpen = false
          }
          if(node.childs.length > 0) {
            pageParams.folder = node
          }else {
            pageParams.node = node
          }
        },

        clickFun(node) {
          if(vm.options.callback) {
            vm.options.callback(node)
          }
          this.defaultClickFun(node)
        }
      },

      template: `
        <div>
          <div @click='clickFun(pnode)' v-if='!pnode.root' class='folder-box'>
            <span v-if='!pnode.isOpen' class='folder-logo'>+</span>
            <span v-if='pnode.isOpen' class='folder-logo'>-</span>
            <p class='folder folder-{{pnode.hightLight}}'>{{pnode ? pnode.name : ''}}</p>
          </div>
          <ul class='tree-ul' v-show='pnode.isOpen'>
            <template v-for='item in list'>
              <tree_item v-if='item.childs.length > 0' :list='item.childs' :pnode='item'></tree_item>
              <li @click='clickFun(item)' v-if='item.childs.length == 0' class='node node-{{item.hightLight}}'>{{ item.name }}</li>
            </template>
          </ul>
        </div>
      `
    }
  },

  created: function() {
    vm = this
    this.initTree()
  }
}

function closeFolder(node) {
  node.isOpen = false
  node.childs.forEach((item) => {
    closeFolder(item)
  })
}

function checkChildNode(comNode, node) {
  if(comNode === node || node.default) {
    return true
  }
  for(let i = 0; i < node.childs.length; ++i) {
    if(checkChildNode(comNode, node.childs[i])) return true
  }
  return false
}

</script>
