<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">信号节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <!-- <div class="panelRow">
        <div>信号名：</div>
        <el-select
          style="width:90%; font-size: 12px"
          placeholder="信号名"
          :value="model.signal"
          :disabled="readOnly"
          @change="
            (e) => {
              onChange('signal', e)
            }
          "
        >
          <el-option v-for="signal in signalDefs" :key="signal.id" :label="signal.name" :value="signal.id" />
        </el-select>
      </div> -->
    </div>
  </div>
</template>
<script>
import DefaultDetail from './DefaultDetail'
export default {
  components: {
    DefaultDetail
  },
  props: {
    model: {
      type: Object,
      default: () => ({})
    },
    onChange: {
      type: Function,
      default: () => {}
    },
    readOnly: {
      type: Boolean,
      default: false
    },
    signalDefs: {
      type: Array,
      default: () => []
    }
  }
}
</script>
