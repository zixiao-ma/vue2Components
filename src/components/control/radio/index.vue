<template>
  <div>
    <el-radio-group v-model="val" @change="handleChangeEvent">
      <el-radio v-for="item in options" :key="item[props.value]" :label="item[props.value]">{{item[props.label]}}</el-radio>
    </el-radio-group>
  </div>
</template>

<script>
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
  }
}
</script>

<style scoped>

</style>
