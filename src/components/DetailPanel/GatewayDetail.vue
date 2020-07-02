<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">
      {{
        model.clazz === 'exclusiveGateway' || model.clazz === 'gateway'
          ? '排他网关'
          : model.clazz === 'parallelGateway'
          ? '并行网关'
          : '包容网关'
      }}
    </div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
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
  }
}
</script>
