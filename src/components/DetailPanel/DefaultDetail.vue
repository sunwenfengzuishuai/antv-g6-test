<template>
  <div>
    <div class="panelRow">
      <div>标题：</div>
      <a-input style="width:90%; font-size:12px" :disabled="readOnly" v-model="inputData" />
    </div>
    <div class="panelRow">
      <a-checkbox
        :checked="!!model.hideIcon"
        @change="(e) => onChange('hideIcon', e.target.checked)"
        :disabled="readOnly"
      >
        隐藏标题
      </a-checkbox>
    </div>
  </div>
</template>
<script>
export default {
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
      inputData: this.model.label
    }
  },
  created() {
    console.log('defaultDetail created')
  },
  watch: {
    inputData(val) {
      this.onChange('label', val)
    },
    model: {
      handler(newval) {
        this.inputData = newval.label
      },
      deep: true
    }
  },
  methods: {}
}
</script>
