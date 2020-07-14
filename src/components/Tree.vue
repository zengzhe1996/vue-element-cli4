<template>
  <div class="tree">
    <el-tree
      icon-class="el-icon-caret-right"
      :data="data"
      :ref="refTree"
      :node-key="nodeKey"
      :props="defaultProps"
      :load="loadNode"
      :lazy="lazy"
      :show-checkbox="showCheckbox"
      :expand-on-click-node="expandOnClickNode"
      :default-expand-all="defaultExpandAll"
      :draggable="draggable"
      :allow-drop="allowDrop"
      :allow-drag="allowDrag"
      @node-drag-start="handleDragStart"
      @node-drag-enter="handleDragEnter"
      @node-drag-leave="handleDragLeave"
      @node-drag-over="handleDragOver"
      @node-drag-end="handleDragEnd"
      @node-drop="handleDrop"
      @node-click="nodeClick"
      @node-contextmenu="nodeRightClick"
      @check="checkNode"
      @check-change="handleCheckChange">
      <span class="custom-tree-node"  slot-scope="{ node, data }">
        <span><i class="icon-tree"></i></span>
        {{node.label}}
      </span>
    </el-tree>
  </div>
</template>

<script>
export default {
  props: {
    defaultProps: {
      type: Object,
      default: () => ({ label: 'name', children: 'children', isLeaf: 'leaf'})
    },
    lazy: {
      type: Boolean,
      default: false
    },
    showCheckbox: {
      type: Boolean,
      default: true
    },
    expandOnClickNode: {
      type: Boolean,
      default: false
    },
    defaultExpandAll: {
      type: Boolean,
      default: false
    },
    draggable: {
      type: Boolean,
      default: true
    },
    nodeKey: {
      type: String,
      default: 'id'
    },
    refTree: {
      type: String,
      default: 'tree'
    }
  },
  data() {
    return {
      count: 3,
      data: [{
          label: '一级 1',
          children: [{
            label: '二级 1-1',
            children: [{
              label: '三级 1-1-1'
            }]
          }]
        }, {
          label: '一级 2',
          children: [{
            label: '二级 2-1',
            children: [{
              label: '三级 2-1-1'
            }]
          }, {
            label: '二级 2-2',
            children: [{
              label: '三级 2-2-1'
            }]
          }]
        }, {
          label: '一级 3',
          children: [{
            label: '二级 3-1',
            children: [{
              label: '三级 3-1-1'
            }]
          }, {
            label: '二级 3-2',
            children: [{
              label: '三级 3-2-1'
            }]
          }]
        }],
    }
  },
  methods: {
    // 懒加载
    loadNode(node, resolve) {
      var data = [{
        name: 'zone' + this.count++,
        id: this.count++,
        leaf: true
      }, {
        name: 'zone' + this.count++,
        id: this.count++,
        leaf: true
      }];
      resolve(data);
      // if (node.level === 0) {
      //   return resolve([{ name: 'region1', id: 1, leaf: true }, { name: 'region2', id: 2, leaf: true}]);
      // }
      // if (node.level > 3) return resolve([]);
      // var hasChild;
      // if (node.data.name === 'region1') {
      //   hasChild = true;
      // } else if (node.data.name === 'region2') {
      //   hasChild = false;
      // } else {
      //   hasChild = Math.random() > 0.5;
      // }
      // setTimeout(() => {
      //   var data;
      //   if (hasChild) {
      //     data = [{
      //       name: 'zone' + this.count++,
      //       id: this.count++,
      //       leaf: true
      //     }, {
      //       name: 'zone' + this.count++,
      //       id: this.count++,
      //       leaf: true
      //     }];
      //   } else {
      //     data = [];
      //   }
      //   resolve(data);
      //   hasChild = false
      // }, 500);
    },
    // 
    handleCheckChange(data, checked, indeterminate) {
      console.log(data, checked, indeterminate);
    },
    // 
    handleNodeClick(data) {
      console.log(data);
    },
    // 节点被点击
    nodeClick(data, node, treeSelf){
      console.log(data)
      console.log(node)
      console.log(treeSelf)
      this.$emit('treeClick', data)
    },
    // 节点右键点击
    nodeRightClick(event, data, node, treeSelf){
      this.$emit('treeRightClick', data)
    },
    // 当节点复选框被点击的时候触发
    checkNode(data, checkedData){
      console.log(data, checkedData)
    },
    // 节点开始拖拽时触发的事件
    handleDragStart(node, ev) {
        // console.log('drag start', node, ev);
    },
    // 拖拽进入其他节点时触发的事件
    handleDragEnter(draggingNode, dropNode, ev) {
      // console.log('tree drag enter: ', dropNode.label);
    },
    // 拖拽离开某个节点时触发的事件
    handleDragLeave(draggingNode, dropNode, ev) {
      // console.log('tree drag leave: ', dropNode.label);
    },
    // 在拖拽节点时触发的事件
    handleDragOver(draggingNode, dropNode, ev) {
      // console.log('tree drag over: ', dropNode.label);
    },
    // 拖拽结束（可能未成功）触发的事件
    handleDragEnd(draggingNode, dropNode, dropType, ev) {
      console.log('tree drag end: ', dropNode && dropNode.label, dropType);
    },
    // 拖拽成功触发事件
    handleDrop(draggingNode, dropNode, dropType, ev) {
      // console.log(draggingNode.data.admId, draggingNode.data , '拖动的元素')
      // console.log(dropNode.data.admId, dropNode.data)
      // console.log('tree drop: ', dropNode.label, dropType);
      let params = {
        dragOrgSort: {
          admId: draggingNode.data.admId,
          admParentId: draggingNode.data.admParentId
        },
        orgSort: {
          admId: dropNode.data.admId,
          admParentId: dropNode.data.admParentId
        },
        top: dropType === 'before' ? true : false
      }
      this.$emit('dropSuccess', draggingNode.data, dropNode.data, dropType, ev)
    },
    // 判断节点能否被拖拽
    allowDrag(draggingNode) {
      return true;
    },
    // 拖拽时判定目标节点能否被放置
    allowDrop(draggingNode, dropNode, type) {
      // console.log(draggingNode, 'draggingNode')
      // console.log(dropNode, 'dropNode')
      if(draggingNode.data.admParentId !== dropNode.data.admParentId) return false
      // 判断节点只能被放置前后位置，不能被插入
      return type === 'inner' ? false : true
    },
  },
  created() {
    console.log(this.draggable)
  },
}
</script>

<style lang="scss" scoped>
  .icon-tree{
    display: inline-block;
    background: url('~@/assets/icons/tree.png') no-repeat center;
    background-size: 100% 100%;
    width: 14px;
    height: 14px;
  }
</style>