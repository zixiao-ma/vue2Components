<template>
  <div>
<!--    <el-select v-model="val" @change="handleChangeEvent" >
      <el-option  v-for="item in options" :key="item[props.value]" :label="item[props.label]" :value="item[props.value]"></el-option>
    </el-select>-->
    <el-checkbox-group v-model="val" @change="handleChangeEvent">
      <el-checkbox v-for="item in options" :key="item[props.value]" :label="item[props.value]">{{item[props.label]}}</el-checkbox>
    </el-checkbox-group>
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
