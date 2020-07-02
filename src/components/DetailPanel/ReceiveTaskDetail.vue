<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">接收节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>等待属性：</div>
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="waitStateInput" />
      </div>
      <div class="panelRow">
        <div>等待值：</div>
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="stateValueInput" />
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
      waitStateInput: this.model.waitState !== undefined ? this.model.waitState : '',
      stateValueInput: this.model.stateValue !== undefined ? this.model.stateValue : ''
    }
  },
  watch: {
    waitStateInput(val) {
      this.onChange('waitState', val)
    },
    stateValueInput(val) {
      this.onChange('stateValue', val)
    },
    model: {
      handler(newval) {
        this.waitStateInput = newval.waitState !== undefined ? newval.waitState : ''
        this.stateValueInput = newval.stateValue !== undefined ? newval.stateValue : ''
      },
      deep: true
    }
  }
}
</script>
