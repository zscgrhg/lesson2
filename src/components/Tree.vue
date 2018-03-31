<script>
  import Node from './Node'

  export default {
    // name: 'ComponentName',
    mounted() {
    },
    props: {
      id: {
        type: String,
        default: 'id'
      },
      label: {
        type: String,
        default: 'label'
      },
      children: {
        type: String,
        default: 'children'
      },
      parent: {
        type: String,
        default: 'parent'
      },
      copy: {
        type: Boolean,
        default: true
      },
      nodeProvider: {
        type: Function,
        default: () => Node
      }
    },
    methods: {
      list2map(arr) {
        let map = {}
        arr.forEach(a => {
          map[a[this.id]] = this.copy ? {...a} : a
        })
        return map
      },
      addNode(nodeArr) {
        var startTime = new Date().getTime()
        console.log(`timer--${new Date().getTime()}`)
        console.log(new Date().getTime())
        let map = this.list2map(nodeArr)
        var roots = []
        Object.keys(map).forEach(k => {
          var v = map[k]
          var parentNode = map[v[this.parent]]
          if (parentNode) {
            parentNode[this.children] = (parentNode[this.children] || []).concat([v])
          } else {
            roots.push(v)
          }
        })
        console.log(`timer--${new Date().getTime()}`)
        console.log(new Date().getTime() - startTime)
        this.roots = (this.roots || []).concat(roots)
        this.nodeMap = Object.assign({}, this.nodeMap, map)
      }
    },
    data() {
      return {
        roots: [],
        nodeMap: {}
      }
    },
    components: {
      Node
    },
    render: function (createElement, hack) {
      // console.log('hack is')
      // console.log(hack)
      // console.log('render start')
      var startTime = new Date().getTime()
      console.log(`render timer--${new Date().getTime()}`)
      let roots = this.roots
      let nodeMap = this.nodeMap
      let nodeComp = this.nodeProvider()
      let nodeRender = function (data) {
        // console.log(data)
        return createElement(nodeComp,
          {
            props: {
              id: data.id,
              roots: roots,
              nodeMap: nodeMap
            },
            slot: 'children'
          },
          (data.children || []).map(item =>
            nodeRender(item, createElement)
          )
        )
      }
      let ele = createElement('div',
        this.roots.map(item =>
          nodeRender(item)
        )
      )
      console.log(`render timer--${new Date().getTime()}`)
      console.log(new Date().getTime() - startTime)
      return ele
    }
  }
</script>

<style>
</style>
