<template>
  <div>
    <ToolbarPanel ref="toolbar" v-if="!isView" />
    <a-row>
      <a-col :xs="2" :sm="3">
        <ItemPanel ref="addItemPanel" v-if="!isView" :height="height" />
      </a-col>
      <a-col :xs="20" :sm="16">
        <div
          ref="canvas"
          class="canvasPanel"
          :style="{ height: height + 'px', width: isView ? '100%' : '100%', 'border-bottom': isView ? 0 : null }"
        ></div>
      </a-col>
      <a-col :xs="2" :sm="5">
        Col
      </a-col>
    </a-row>
  </div>
</template>

<script>
import G6 from '@antv/g6/src'
import { getShapeName } from '../util/clazz'
import Command from '../plugins/command'
import Toolbar from '../plugins/toolbar'
import AddItemPanel from '../plugins/addItemPanel'
import CanvasPanel from '../plugins/canvasPanel'
import ToolbarPanel from '../components/ToolbarPanel'
import ItemPanel from '../components/ItemPanel'
import { exportXML } from '../util/bpmn'
import registerShape from '../shape'
import registerBehavior from '../behavior'
registerShape(G6)
registerBehavior(G6)
// @ is an alias to /src

export default {
  name: 'Home',
  props: {
    isView: {
      type: Boolean,
      default: false
    },
    mode: {
      type: String,
      default: 'edit'
    },
    height: {
      type: Number,
      default: 500
    },
    lang: {
      type: String,
      default: 'zh'
    },
    data: {
      type: Object,
      default: () => ({
        nodes: [
          { id: 'startNode1', x: 50, y: 200, label: '', clazz: 'start' },
          { id: 'startNode2', x: 50, y: 320, label: '', clazz: 'timerStart' },
          { id: 'taskNode1', x: 200, y: 200, label: '主任审批', clazz: 'userTask' },
          { id: 'taskNode2', x: 400, y: 200, label: '经理审批', clazz: 'scriptTask' },
          { id: 'gatewayNode', x: 400, y: 320, label: '金额大于1000', clazz: 'inclusiveGateway' },
          { id: 'taskNode3', x: 400, y: 450, label: '董事长审批', clazz: 'receiveTask' },
          { id: 'catchNode1', x: 600, y: 200, label: '等待结束', clazz: 'signalCatch' },
          { id: 'endNode', x: 600, y: 320, label: '', clazz: 'end' }
        ],
        edges: [
          { source: 'startNode1', target: 'taskNode1', sourceAnchor: 1, targetAnchor: 3, clazz: 'flow' },
          { source: 'startNode2', target: 'gatewayNode', sourceAnchor: 1, targetAnchor: 3, clazz: 'flow' },
          { source: 'taskNode1', target: 'catchNode1', sourceAnchor: 0, targetAnchor: 0, clazz: 'flow' },
          { source: 'taskNode1', target: 'taskNode2', sourceAnchor: 1, targetAnchor: 3, clazz: 'flow' },
          { source: 'taskNode2', target: 'gatewayNode', sourceAnchor: 1, targetAnchor: 0, clazz: 'flow' },
          { source: 'taskNode2', target: 'taskNode1', sourceAnchor: 2, targetAnchor: 2, clazz: 'flow' },
          { source: 'gatewayNode', target: 'taskNode3', sourceAnchor: 2, targetAnchor: 0, clazz: 'flow' },
          { source: 'gatewayNode', target: 'endNode', sourceAnchor: 1, targetAnchor: 2, clazz: 'flow' },
          { source: 'taskNode3', target: 'endNode', sourceAnchor: 1, targetAnchor: 1, clazz: 'flow' },
          { source: 'catchNode1', target: 'endNode', sourceAnchor: 1, targetAnchor: 0, clazz: 'flow' }
        ]
      })
    },
    users: {
      type: Array,
      default: () => []
    },
    groups: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      resizeFunc: () => {},
      selectedModel: {},
      processModel: {
        id: '',
        name: '',
        clazz: 'process',
        dataObjs: [],
        signalDefs: [],
        messageDefs: []
      },
      graph: null,
      cmdPlugin: null
    }
  },
  components: { ToolbarPanel, ItemPanel },
  methods: {
    initShape(data) {
      console.log('initShape')
      if (data && data.nodes) {
        return {
          nodes: data.nodes.map((node) => {
            return {
              shape: getShapeName(node.clazz),
              ...node
            }
          }),
          edges: data.edges
        }
      }
      return data
    },
    initEvents() {
      console.log('initEvents')
      this.graph.on('afteritemselected', (items) => {
        if (items && items.length > 0) {
          const item = this.graph.findById(items[0])
          this.selectedModel = { ...item.getModel() }
        } else {
          this.selectedModel = this.processModel
        }
      })
      const page = this.$refs['canvas']
      const graph = this.graph
      const height = this.height - 1
      this.resizeFunc = () => {
        graph.changeSize(page.offsetWidth, height)
      }
      window.addEventListener('resize', this.resizeFunc)
    }
  },
  mounted() {
    console.log(111)
    let plugins = []
    if (!this.isView) {
      console.log(222)
      this.cmdPlugin = new Command()
      const toolbar = new Toolbar({ container: this.$refs['toolbar'].$el })
      const addItemPanel = new AddItemPanel({ container: this.$refs['addItemPanel'].$el })
      const canvasPanel = new CanvasPanel({ container: this.$refs['canvas'] })
      plugins = [this.cmdPlugin, toolbar, addItemPanel, canvasPanel]
    }
    const width = this.$refs['canvas'].offsetWidth
    this.graph = new G6.Graph({
      plugins: plugins,
      container: this.$refs['canvas'],
      height: this.height,
      width: width,
      modes: {
        default: ['drag-canvas', 'clickSelected'],
        view: [],
        edit: [
          'drag-canvas',
          'hoverNodeActived',
          'hoverAnchorActived',
          'dragNode',
          'dragEdge',
          'dragPanelItemAddNode',
          'clickSelected',
          'deleteItem',
          'itemAlign'
        ]
      },
      defaultEdge: {
        shape: 'flow-polyline-round'
      }
    })
    this.graph.saveXML = (createFile = true) => exportXML(this.graph.save(), this.processModel, createFile)
    if (this.isView) this.graph.setMode('view')
    else this.graph.setMode(this.mode)
    this.graph.data(this.initShape(this.data))
    this.graph.render()
    if (this.isView && this.data && this.data.nodes) {
      this.graph.fitView(5)
    }
    this.initEvents()
    this.$refs.addItemPanel.activeKeys = []
  }
}
</script>
