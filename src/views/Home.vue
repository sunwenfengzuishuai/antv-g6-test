<template>
  <div>
    <ToolbarPanel ref="toolbar" />
    <a-row>
      <a-col :xs="2" :sm="4">
        Col
      </a-col>
      <a-col :xs="20" :sm="16">
        Col
      </a-col>
      <a-col :xs="2" :sm="4">
        Col
      </a-col>
    </a-row>
  </div>
</template>

<script>
import ToolbarPanel from '../components/ToolbarPanel'
// @ is an alias to /src

export default {
  name: 'Home',
  components: { ToolbarPanel }
}
</script>
