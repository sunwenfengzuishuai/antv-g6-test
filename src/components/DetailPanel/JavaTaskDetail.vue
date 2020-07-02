<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">自定义类节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>类名：</div>
        <!-- <el-input
          style="width:90%; font-size:12px"
          :disabled="readOnly"
          :value="model.javaClass"
          @input="
            (value) => {
              onChange('javaClass', value)
            }
          "
        /> -->
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="javaClassInput" />
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
      javaClassInput: this.model.javaClass !== undefined ? this.model.javaClass : ''
    }
  },
  watch: {
    javaClassInput(val) {
      this.onChange('javaClass', val)
    },
    model: {
      handler(newval) {
        this.javaClassInput = newval.javaClass !== undefined ? newval.javaClass : ''
      },
      deep: true
    }
  }
}
</script>
