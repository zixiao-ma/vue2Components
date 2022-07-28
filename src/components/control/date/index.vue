<template>
  <div>
    <el-date-picker
      v-model="val"
      :type="config.model || 'date'"
      :placeholder="config.placeholder || '请选择日期'"
      :range-separator="config.rangeSeparator || '-'"
      :start-placeholder="config.startPlaceholder || '开始日期'"
      :end-placeholder="config.endPlaceholder || '结束日期'"
      @change="handleChangeEvent"
      :picker-options="pickerOptions()"
    >
    </el-date-picker>
  </div>
</template>

<script >
import { props, mixin } from '@/components/control/basis'

export default {
  name: 'SelectComponent',
  mixins: [mixin],
  props: {
    ...props
  },
  data () {
    return {
      val: ''
    }
  },
  methods: {
    pickerOptions () {
      const disabledDate = this.config.disabledDate
      const disabledToday = this.config.disabledToday
      const disabledDateRules = this.config.disabledDateRules && Object.prototype.toString.call(this.config.disabledDateRules) === '[object Function]'
      return {
        disabledDate: (time) => {
          if (disabledDateRules) {
            return this.config.disabledDateRules(time)
          } else if (disabledDate) {
            return time.getTime() < new Date() - 8.64e7
          } else if (disabledToday) {
            return time.getTime() < new Date()
          } else {
            return false
          }
        }
      }
    }
  }
}
</script>
<style lang="scss" scoped>

</style>
