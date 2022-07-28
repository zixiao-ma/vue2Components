<template>
  <div class="relative">
    <el-input v-model="val" @input="handleChangeEvent"></el-input>
    <div class="code-button" v-if="config.valueType === 'sendcode'">
      <el-button size="mini" :disabled="disabled" :loading="loading" type="primary" @click="getSms">{{text}}</el-button>
    </div>
  </div>
</template>

<script>
import { props, mixin } from '../basis'
export default {
  name: 'InputComponent',
  mixins: [mixin],
  props: {
    ...props
  },
  watch: {
    value: {
      handler (newValue) {
        console.log(newValue)
        this.val = newValue
      },
      immediate: true
    }
  },
  data () {
    return {
      val: '',
      text: '获取验证码',
      loading: false,
      disabled: false,
      s: 3,
      timer: null
    }
  },
  methods: {
    handleChangeEvent (value) {
      this.$emit('update:value', value)
      if (this.config.callback && Object.prototype.toString.call(this.config.callback) === '[object Function]') {
        this.config.callback(value)
      }
    },
    getSms () {
      if (!this.config.sendAccount) {
        this.$message.error('请输入手机号')
        return false
      }
      if (this.config.beforeChange && Object.prototype.toString.call(this.config.beforeChange) === '[object Function]') {
        this.text = '发送中'
        this.loading = true
        this.config.beforeChange().then(response => {
          this.text = `${this.s}秒后重试`
          this.loading = false
          this.disabled = true
          this.countDown()
        }).catch(error => {
          this.text = '重新获取'
          this.disabled = false
          console.log(error)
        })
      }
      console.log(this.config.sendAccount)
    },
    countDown () {
      if (this.timer) {
        clearInterval(this.timer)
      }
      this.timer = setInterval(() => {
        this.s--
        if (this.s === 0) {
          this.text = '重新获取'
          this.disabled = false
          this.s = 60
          clearInterval(this.timer)
          this.timer = null
        } else {
          this.text = `${this.s}秒后重试`
        }
      }, 1000)
    }
  }
}
</script>

<style lang="scss" scoped>
.relative{
  position: relative;
}
.code-button{
  position: absolute;
  right: 10px;
  top: 0;
}
</style>
