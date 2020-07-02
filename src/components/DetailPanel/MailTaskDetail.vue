<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">邮件节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>收件人：</div>
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="toInput" />
      </div>
      <div class="panelRow">
        <div>标题：</div>
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="subjectInput" />
      </div>
      <div class="panelRow">
        <div>内容：</div>
        <a-textarea
          style="width:90%; font-size:12px"
          v-model="contentInput"
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
      toInput: this.model.to !== undefined ? this.model.to : '',
      subjectInput: this.model.subject !== undefined ? this.model.subject : '',
      contentInput: this.model.content !== undefined ? this.model.content : ''
    }
  },
  watch: {
    toInput(val) {
      this.onChange('to', val)
    },
    subjectInput(val) {
      this.onChange('subject', val)
    },
    contentInput(val) {
      this.onChange('content', val)
    },
    model: {
      handler(newval) {
        this.toInput = newval.to !== undefined ? newval.to : ''
        this.subjectInput = newval.subject !== undefined ? newval.subject : ''
        this.contentInput = newval.content !== undefined ? newval.content : ''
      },
      deep: true
    }
  }
}
</script>
