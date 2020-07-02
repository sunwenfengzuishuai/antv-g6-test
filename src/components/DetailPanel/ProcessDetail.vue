<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">流程</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>流程标识：</div>
        <!-- <el-input
          style="width:90%; font-size:12px"
          :disabled="readOnly"
          :value="model.id"
          @input="
            (value) => {
              onChange('id', value)
            }
          "
        /> -->
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="idInput" />
      </div>
      <div class="panelRow">
        <div>流程名称：</div>
        <!-- <el-input
          style="width:90%; font-size:12px"
          :disabled="readOnly"
          :value="model.name"
          @input="
            (value) => {
              onChange('name', value)
            }
          "
        /> -->
        <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="nameInput" />
      </div>
      <div class="panelRow">
        <div>
          数据对象：
          <a-button :disabled="readOnly" size="small" @click="() => {}">编辑</a-button>
        </div>
      </div>
      <div class="panelRow">
        <div>
          信号定义：
          <a-button :disabled="readOnly" size="small" @click="() => {}">编辑</a-button>
        </div>
      </div>
      <div class="panelRow">
        <div>
          消息定义：
          <a-button :disabled="readOnly" size="small" @click="() => {}">编辑</a-button>
        </div>
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
      idInput: this.model.id,
      nameInput: this.model.name
    }
  },
  watch: {
    idInput(val) {
      this.onChange('id', val)
    },
    nameInput(val) {
      this.onChange('name', val)
    }
    // model: {
    //   handler(newval) {
    //     console.log(newval)
    //     this.idInput = newval.id
    //     this.nameInput = newval.name
    //   },
    //   deep: true
    // }
  }
}
</script>
