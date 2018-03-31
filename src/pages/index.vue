<template>
  <q-page>
    <h3>这是一棵树</h3>
    <a href="javascript:void(0)" @click="onClick">添加节点</a>
    <tree ref="tree" :node-provider="nodeProvider"/>
  </q-page>
</template>

<style>
</style>

<script>
  import Tree from '../components/Tree'
  import NewNode from '../components/NewNode'

  export default {
    mounted() {
      this.$axios.get('http://localhost/nodes?max=10000&mod=10', {})
        .then(res => {
          this.nodeArr = res.data
        })
    },
    data: function () {
      return {
        nodeArr: []
      }
    },
    components: {Tree, NewNode},
    methods: {
      onClick() {
        console.log(this)
        this.$refs.tree.addNode(this.nodeArr)
      },
      nodeProvider: () => NewNode
    }
  }
</script>
