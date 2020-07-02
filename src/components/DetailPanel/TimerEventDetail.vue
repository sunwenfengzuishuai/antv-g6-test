<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">定时节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>循环时间：</div>
        <!-- <el-input
          style="width:90%; font-size:12px"
          type="textarea"
          :rows="4"
          :disabled="readOnly"
          :value="model.cycle"
          @input="
            (value) => {
              onChange('cycle', value)
            }
          "
        /> -->
        <a-textarea
          style="width:90%; font-size:12px"
          v-model="cycleInput"
          :disabled="readOnly"
          :auto-size="{ minRows: 3, maxRows: 5 }"
        />
      </div>
      <div class="panelRow">
        <div>持续时间：</div>
        <!-- <el-input
          style="width:90%; font-size:12px"
          type="textarea"
          :rows="4"
          :disabled="readOnly"
          :value="model.duration"
          @input="
            (value) => {
              onChange('duration', value)
            }
          "
        /> -->
        <a-textarea
          style="width:90%; font-size:12px"
          v-model="durationInput"
          :disabled="readOnly"
          :auto-size="{ minRows: 3, maxRows: 5 }"
        />
      </div>
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
    }
  },
  data() {
    return {
      cycleInput: this.model.cycle !== undefined ? this.model.cycle : '',
      durationInput: this.model.duration !== undefined ? this.model.duration : ''
    }
  },
  watch: {
    cycleInput(val) {
      this.onChange('cycle', val)
    },
    durationInput(val) {
      this.onChange('duration', val)
    },
    model: {
      handler(newval) {
        this.cycleInput = newval.cycle !== undefined ? newval.cycle : ''
        this.durationInput = newval.duration !== undefined ? newval.duration : ''
      },
      deep: true
    }
  }
}
</script>
