<template>
  <q-page>
    <h3>这是一棵树</h3>
    <a href="javascript:void(0)" @click="onClick">添加节点</a>
    <tree ref="tree"/>
  </q-page>
</template>

<style>
</style>

<script>
  import Tree from '../components/Tree'

  export default {
    mounted() {
      this.$axios.get('http://localhost/nodes?max=10000', {})
        .then(res => {
          this.nodeArr = res.data
        })
    },
    data: function () {
      return {
        nodeArr: []
      }
    },
    components: {Tree},
    methods: {
      onClick() {
        this.$refs.tree.addNode(this.nodeArr)
      }
    }
  }
</script>
