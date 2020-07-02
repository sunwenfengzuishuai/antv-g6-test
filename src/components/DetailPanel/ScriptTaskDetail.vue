<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">脚本节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>脚本：</div>
        <a-textarea
          style="width:90%; font-size:12px"
          v-model="scriptInput"
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
      scriptInput: this.model.script !== undefined ? this.model.script : ''
    }
  },
  watch: {
    scriptInput(val) {
      this.onChange('script', val)
    },
    model: {
      handler(newval) {
        this.scriptInput = newval.script !== undefined ? newval.script : ''
      },
      deep: true
    }
  }
}
</script>
