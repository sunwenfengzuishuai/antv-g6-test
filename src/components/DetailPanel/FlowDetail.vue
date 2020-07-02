<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">连接线</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>条件表达式：</div>
        <a-textarea
          style="width:90%; font-size:12px"
          v-model="conditionExpressionInput"
          :disabled="readOnly"
          :auto-size="{ minRows: 3, maxRows: 5 }"
        />
      </div>
      <div class="panelRow">
        <div>序号：</div>
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="seqInput" />
      </div>
      <div class="panelRow">
        <a-checkbox
          :checked="!!model.reverse"
          @change="(e) => onChange('reverse', e.target.checked)"
          :disabled="readOnly"
        >
          反向
        </a-checkbox>
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
      seqInput: this.model.seq !== undefined ? this.model.seq : '',
      conditionExpressionInput: this.model.conditionExpression !== undefined ? this.model.conditionExpression : ''
    }
  },
  watch: {
    seqInput(val) {
      this.onChange('seq', val)
    },
    conditionExpressionInput(val) {
      this.onChange('conditionExpression', val)
    },
    model: {
      handler(newval) {
        this.seqInput = newval.seq !== undefined ? newval.seq : ''
        this.conditionExpressionInput = newval.conditionExpression !== undefined ? newval.conditionExpression : ''
      },
      deep: true
    }
  }
}
</script>
