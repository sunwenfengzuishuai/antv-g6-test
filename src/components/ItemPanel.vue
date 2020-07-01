<template>
  <div class="itemPanel">
    <a-collapse v-model="activeKeys">
      <a-collapse-panel key="1" header="开始事件">
        <img
          data-item="{clazz:'start',size:'30*30',label:''}"
          :src="require('../assets/flow/start.svg')"
          style="width:42px;height:42px"
        />
        <div>开始节点</div>
        <img
          data-item="{clazz:'timerStart',size:'30*30',label:''}"
          :src="require('../assets/flow/timer-start.svg')"
          style="width:42px;height:42px"
        />
        <div>定时节点</div>
        <img
          data-item="{clazz:'messageStart',size:'30*30',label:''}"
          :src="require('../assets/flow/message-start.svg')"
          style="width:42px;height:42px"
        />
        <div>消息节点</div>
        <img
          data-item="{clazz:'signalStart',size:'30*30',label:''}"
          :src="require('../assets/flow/signal-start.svg')"
          style="width:42px;height:42px"
        />
        <div>信号节点</div>
      </a-collapse-panel>
      <a-collapse-panel key="2" header="活动">
        <img :data-item="userTaskData" :src="require('../assets/flow/user-task.svg')" style="width:80px;height:44px" />
        <div>审批节点</div>
        <img
          :data-item="scriptTaskData"
          :src="require('../assets/flow/script-task.svg')"
          style="width:80px;height:44px"
        />
        <div>脚本节点</div>
        <img :data-item="javaTaskData" :src="require('../assets/flow/java-task.svg')" style="width:80px;height:44px" />
        <div>自定义类节点</div>
        <img :data-item="mailTaskData" :src="require('../assets/flow/mail-task.svg')" style="width:80px;height:44px" />
        <div>邮件节点</div>
        <img
          :data-item="receiveTaskData"
          :src="require('../assets/flow/receive-task.svg')"
          style="width:80px;height:44px"
        />
        <div>接收节点</div>
      </a-collapse-panel>
      <a-collapse-panel key="3" header="网关">
        <img
          data-item="{clazz:'exclusiveGateway',size:'40*40',label:''}"
          :src="require('../assets/flow/exclusive-gateway.svg')"
          style="width:48px;height:48px"
        />
        <div>排他网关</div>
        <img
          data-item="{clazz:'parallelGateway',size:'40*40',label:''}"
          :src="require('../assets/flow/parallel-gateway.svg')"
          style="width:48px;height:48px"
        />
        <div>并行网关</div>
        <img
          data-item="{clazz:'inclusiveGateway',size:'40*40',label:''}"
          :src="require('../assets/flow/inclusive-gateway.svg')"
          style="width:48px;height:48px"
        />
        <div>包含网关</div>
      </a-collapse-panel>
      <a-collapse-panel key="4" header="捕获事件">
        <img
          data-item="{clazz:'timerCatch',size:'50*30',label:''}"
          :src="require('../assets/flow/timer-catch.svg')"
          style="width:58px;height:38px"
        />
        <div>定时节点</div>
        <img
          data-item="{clazz:'messageCatch',size:'50*30',label:''}"
          :src="require('../assets/flow/message-catch.svg')"
          style="width:58px;height:38px"
        />
        <div>消息节点</div>
        <img
          data-item="{clazz:'signalCatch',size:'50*30',label:''}"
          :src="require('../assets/flow/signal-catch.svg')"
          style="width:58px;height:38px"
        />
        <div>信号节点</div>
      </a-collapse-panel>
      <a-collapse-panel key="5" header="结束事件">
        <img
          data-item="{clazz:'end',size:'30*30',label:''}"
          :src="require('../assets/flow/end.svg')"
          style="width:42px;height:42px"
        />
        <div>结束节点</div>
      </a-collapse-panel>
    </a-collapse>
  </div>
</template>
<script>
export default {
  props: {
    height: {
      type: Number,
      default: 800
    }
  },
  data() {
    return {
      activeKeys: ['1', '2', '3', '4', '5'],
      userTaskData: "{clazz:'userTask',size:'80*44',label:'" + '审批节点' + "'}",
      scriptTaskData: "{clazz:'scriptTask',size:'80*44',label:'" + '脚本节点' + "'}",
      javaTaskData: "{clazz:'javaTask',size:'80*44',label:'" + '自定义类节点' + "'}",
      mailTaskData: "{clazz:'mailTask',size:'80*44',label:'" + '邮件节点' + "'}",
      receiveTaskData: "{clazz:'receiveTask',size:'80*44',label:'" + '接收节点' + "'}"
    }
  }
}
</script>
<style scoped>
.itemPanel img:hover {
  border: 1px solid #ccc;
  cursor: move;
}
</style>
