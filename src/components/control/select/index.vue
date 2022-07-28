<template>
  <div>
    <el-select v-model="val" @change="handleChangeEvent" >
      <el-option  v-for="item in options" :key="item[props.value]" :label="item[props.label]" :value="item[props.value]"></el-option>
    </el-select>
  </div>
</template>

<script>
import { mixin, props } from '@/components/control/basis'
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
