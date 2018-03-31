<template>
  <div :id="id" v-show="visible">
    <span style="width: 5%">
      <span v-if="!isLeaf">
        <a href="javascript:void(0)" @click="()=>expanded=!expanded">
          <i class="material-icons" v-if="!expanded">&#xE315;</i>
          <i class="material-icons" v-else>&#xE313;</i>
        </a>
      </span>
      <span v-else>----</span>
    </span>
    <strong v-bind:class="{ 'bg-brown-3': !isLeaf, 'bg-light-green-9': isLeaf}"
            @click="echo">{{nodeMap[id].label}}</strong>
    <div class="node-child" v-show="expanded" v-if="!isLeaf">
      <slot name="children"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    mounted() {
      // console.log(`node mounted done ${this.id}`)
    },
    props: {
      id: Number,
      roots: Array,
      nodeMap: Object
    },
    data() {
      return {
        expanded: false,
        visible: true
      }
    },
    computed: {
      isLeaf: function () {
        return !this.nodeMap[this.id].children
      }
    },
    methods: {
      echo() {
        // console.log(this.id)
      }
    }
  }
</script>

<style>
  .node-child {
    margin-left: 20px;
  }
</style>
