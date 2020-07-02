<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">消息节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <!-- <div class="panelRow">
        <div>消息名：</div>
        <el-select
          style="width:90%; font-size: 12px"
          :placeholder="消息名"
          :value="model.message"
          :disabled="readOnly"
          @change="
            (e) => {
              onChange('message', e)
            }
          "
        >
          <el-option v-for="message in messageDefs" :key="message.id" :label="message.name" :value="message.id" />
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
    messageDefs: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      messageInput: this.model.message !== undefined ? this.model.message : ''
    }
  },
  watch: {
    messageInput(val) {
      this.onChange('message', val)
    },
    model: {
      handler(newval) {
        this.messageInput = newval.message !== undefined ? newval.message : ''
      },
      deep: true
    }
  }
}
</script>
