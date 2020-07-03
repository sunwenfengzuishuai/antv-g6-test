<template>
  <div :data-clazz="model.clazz">
    <div class="panelTitle">审批节点</div>
    <div class="panelBody">
      <DefaultDetail :model="model" :onChange="onChange" :readOnly="readOnly" />
      <div class="panelRow">
        <div>指派类型：</div>
        <a-select
          placeholder="选择一个类型"
          style="width:90%; font-size: 12px"
          v-model="assignTypeSelect"
          @change="
            (e) => {
              console(e)
              onChange('assignValue', [])
              onChange('assignType', e)
            }
          "
        >
          <a-select-option value="assignee">
            受理人
          </a-select-option>
          <a-select-option value="person">
            候选人
          </a-select-option>
          <a-select-option value="persongroup">
            候选组
          </a-select-option>
        </a-select>
      </div>
      <div v-if="model.assignType === 'assignee'" class="panelRow">
        <div>受理人：</div>
        <a-select
          placeholder="受理人"
          style="width:90%; font-size: 12px"
          v-model="assigneeSelect"
          @change="
            (e) => {
              onChange('assignValue', [e])
            }
          "
        >
          <a-select-option v-for="user in users" :key="user.id" :value="user.id"> {{ user.name }}</a-select-option>
        </a-select>
      </div>
      <div v-if="model.assignType === 'person'" class="panelRow">
        <div>候选人：</div>
        <a-select
          placeholder="候选人"
          style="width:90%; font-size: 12px"
          v-model="personSelect"
          mode="multiple"
          @change="
            (e) => {
              onChange('assignValue', e)
            }
          "
        >
          <a-select-option v-for="user in users" :key="user.id" :value="user.id"> {{ user.name }}</a-select-option>
        </a-select>
      </div>
      <div v-else-if="model.assignType === 'persongroup'" class="panelRow">
        <div>候选组：</div>
        <a-select
          placeholder="候选组"
          style="width:90%; font-size: 12px"
          v-model="persongroupSelect"
          mode="multiple"
          @change="
            (e) => {
              onChange('assignValue', e)
            }
          "
        >
          <a-select-option v-for="user in users" :key="user.id" :value="user.id"> {{ user.name }}</a-select-option>
        </a-select>
      </div>
      <div class="panelRow">
        <div style="display:inline">到期时间：</div>
        <a-date-picker
          style="width:90%; min-width:null"
          showTime
          placeholder="请选择日期"
          :disabled="readOnly"
          :format="dateFormat"
          :value="model.dueDate ? moment(model.dueDate) : null"
          @change="
            (date) => {
              console(date)
              console(typeof date)
              if (date) {
                onChange('dueDate', momentToTimestamp(date))
              } else {
                onChange('dueDate', date)
              }
            }
          "
        />
        <br />
      </div>
      <div class="panelRow">
        <a-checkbox
          @change="(e) => onChange('isSequential', e.target.checked)"
          :disabled="readOnly"
          :checked="!!model.isSequential"
          >会签</a-checkbox
        >
      </div>
    </div>
  </div>
</template>
<script>
import moment from 'moment'
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
    users: {
      type: Array,
      default: () => []
    },
    groups: {
      type: Array,
      default: () => []
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
      assignTypeSelect: this.model.assignType !== undefined ? this.model.assignType : '', //类型
      assigneeSelect: '', //受理人
      personSelect: [], //候选人
      persongroupSelect: [], //候选组
      moment: '',
      dateFormat: 'YYYY/MM/DD HH:mm:ss'
    }
  },
  methods: {
    console(val) {
      console.log(val)
    },
    momentToTimestamp(date) {
      console.log(date)
      //return new Date(date._d).getTime()
      return date.format(this.dateFormat)
    }
  },
  created() {
    this.moment = moment
    if (this.model.assignType !== undefined) {
      this.assignTypeSelect = this.model.assignType
      if (this.model.assignType === 'assignee') {
        this.assigneeSelect = this.model.assignValue[0] !== undefined ? this.model.assignValue[0] : ''
      } else if (this.model.assignType === 'person') {
        this.personSelect = this.model.assignValue
      } else if (this.model.assignType === 'persongroup') {
        this.persongroupSelect = this.model.assignValue
      }
    } else {
      this.assignTypeSelect = ''
    }
  },
  watch: {
    model: {
      handler(val) {
        console.log('model watch 变了')
        if (val.assignType !== undefined) {
          this.assignTypeSelect = val.assignType
          if (val.assignType === 'assignee') {
            this.assigneeSelect = val.assignValue[0] !== undefined ? val.assignValue[0] : ''
          } else if (val.assignType === 'person') {
            this.personSelect = val.assignValue
          } else if (val.assignType === 'persongroup') {
            this.persongroupSelect = val.assignValue
          }
        } else {
          this.assignTypeSelect = ''
        }
      },
      deep: true
    }
  }
}
</script>
