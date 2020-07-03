<template>
  <div>
    <a-button
      v-if="!isView"
      style="float:right;margin-top:6px;margin-right:6px;"
      @click="
        () => {
          graph.saveXML()
        }
      "
      >导出xml</a-button
    >
    <a-button
      v-if="!isView"
      style="float:right;margin-top:6px;margin-right:6px;"
      @click="
        () => {
          graph.saveImg()
        }
      "
      >导出图片</a-button
    >
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
        <DetailPanel
          ref="detailPanel"
          v-if="!isView"
          :height="height"
          :model="selectedModel"
          :readOnly="mode !== 'edit'"
          :users="users"
          :groups="groups"
          :signalDefs="processModel.signalDefs"
          :messageDefs="processModel.messageDefs"
          :onChange="
            (key, val) => {
              onItemCfgChange(key, val)
            }
          "
        />
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
import { exportXML, exportImg } from '../util/bpmn'
import DetailPanel from '../components/DetailPanel'
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
      default: () => [
        { id: '1', name: '张三' },
        { id: '2', name: '李四' },
        { id: '3', name: '王五' },
        { id: '4', name: '赵六' }
      ]
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
  components: { ToolbarPanel, ItemPanel, DetailPanel },
  methods: {
    initShape(data) {
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
    },
    onItemCfgChange(key, value) {
      const items = this.graph.get('selectedItems')
      if (items && items.length > 0) {
        //其他节点属性面板
        const item = this.graph.findById(items[0])
        if (this.graph.executeCommand) {
          this.graph.executeCommand('update', {
            itemId: items[0],
            updateModel: { [key]: value }
          })
        } else {
          this.graph.updateItem(item, { [key]: value })
        }
        this.selectedModel = { ...item.getModel() }
      } else {
        //主流程属性面板
        const canvasModel = { ...this.processModel, [key]: value }
        this.selectedModel = canvasModel
        this.processModel = canvasModel
      }
    }
  },
  mounted() {
    let plugins = []
    if (!this.isView) {
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
    this.graph.saveImg = (createFile = true) => exportImg(this.$refs['canvas'], this.processModel.name, createFile)
    if (this.isView) this.graph.setMode('view')
    else this.graph.setMode(this.mode)
    this.graph.data(this.initShape(this.data))
    this.graph.render()
    if (this.isView && this.data && this.data.nodes) {
      this.graph.fitView(5)
    }
    this.initEvents()
    this.$refs.addItemPanel.activeKeys = []
    // const data = this.graph.save()
    // data.nodes[0].label = '买了否冷'
    // data.nodes[2].dueDate = '2020/07/03 17:12:35'
    // data.nodes[2].active = true
    // setTimeout(() => {
    //   console.log('更新数据')
    //   this.graph.data(data)
    //   this.graph.render()
    //   this.graph.fitView(50)
    // }, 5000)
  },
  destroyed() {
    window.removeEventListener('resize', this.resizeFunc)
    this.graph.getNodes().forEach((node) => {
      node.getKeyShape().stopAnimate()
    })
  }
}
</script>
