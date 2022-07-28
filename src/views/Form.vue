<template>
  <div>
    <yang-form :item="formItem" :field="formField" :button="formButton" :before-submit="handleBeforeSubmit"></yang-form>
  </div>
</template>

<script>
export default {
  name: 'Form',
  data () {
    return {
      formButton: [
        { label: '提交', key: 'submit', type: 'primary' },
        { label: '重置', key: 'cancel', type: 'danger' },
        { label: '下一步', key: 'next', type: 'success' }
      ],
      formItem: [
        {
          label: '手机号',
          type: 'input',
          valueType: 'phone',
          prop: 'phone',
          required: true
        },
        {
          label: '验证码',
          type: 'input',
          sendAccount: '',
          valueType: 'sendcode',
          prop: 'code',
          required: true,
          beforeChange: () => {
            return this.getApiSms()
          }
        },
        {
          label: '文件上传',
          type: 'upload',
          prop: 'file',
          model: 'card',
          width: '90px',
          height: '90px',
          // 上传的文件地址
          url: '/api/upload_single',
          // 上传方式
          method: 'post',
          // 是否多选
          multiple: true,
          // 最多文件条数
          limit: 2,
          // 单个文件大小MB
          maxSize: 2,
          // 显示上传文件的列表
          showFile: true,
          // 是否圆角
          isRound: true
        },
        {
          label: '滑动开关',
          type: 'switch',
          prop: 'status',
          activeValue: 1,
          inactiveValue: 0,
          beforeChange: () => {
            return this.handleSwitchApi()
          }
        },
        {
          label: '日期',
          type: 'date',
          model: 'datetimerange',
          rangeSeparator: '至',
          placeholder: '请选择您的生日',
          disabledDateRules: (time) => {
            return time.getTime() > new Date() - 86400000 * 1
          },
          format: 'yyyy-MM',
          valueFormat: 'yyyy-MM-dd HH:mm:ss',
          // disabledDate: true,
          // disabledToDay: true,
          startPlaceholder: '请选择开始创建的日期',
          endPlaceholder: '请选择结束创建的日期',
          prop: 'createDate',
          required: true
        },
        {
          label: '交通工具',
          type: 'radio',
          prop: 'car',
          required: true,
          props: {
            label: 'a',
            value: 'b'
          },
          options: [
            {
              a: '火车',
              b: 1
            },
            {
              a: '高铁',
              b: 2
            },
            {
              a: '飞机',
              b: 3
            }
          ]
        },
        {
          label: '食物',
          type: 'checkbox',
          prop: 'food',
          required: true,
          props: {
            label: 'a',
            value: 'b'
          },
          options: [
            {
              a: '苹果',
              b: 1
            },
            {
              a: '西瓜',
              b: 2
            },
            {
              a: '芒果',
              b: 3
            },
            {
              a: '哈密瓜',
              b: 4
            }
          ]
        },
        {
          label: '教室',
          type: 'select',
          prop: 'class_room',
          required: true,
          props: {
            label: 'a',
            value: 'b'
          },
          options: [
            {
              a: '一教',
              b: 1
            },
            {
              a: '二教',
              b: 2
            },
            {
              a: '三教',
              b: 3
            },
            {
              a: '四教',
              b: 4
            }
          ]
        },
        {
          label: '教室1',
          type: 'select',
          prop: 'class_room1',
          // required: true,
          props: {
            label: 'class_name',
            value: 'id'
          },
          // initRequest: true,
          url: '/classroom/',
          method: 'GET'
        }
      ],
      formField: {
        phone: '',
        password: '',
        age: '',
        email: '',
        food: [1, 4],
        car: 1,
        createDate: '',
        status: 1,
        file: ''
      }
    }
  },
  watch: {
    'formField.phone': {
      handler: function (val, oldVal) {
        console.log(val)
        this.formItem[1].sendAccount = val
      },
      immediate: true,
      deep: true
    }
  },
  components: {
    yangForm: () => import('../components/form/index')
  },
  methods: {
    handleBeforeSubmit () {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          console.log(this.formField)
          resolve()
          // eslint-disable-next-line prefer-promise-reject-errors
          // reject()
        }, 2000)
      })
    },
    handleSwitchApi () {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve()
        }, 2000)
      })
    },
    getApiSms () {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve()
        }, 2000)
      })
    }
  }
}
</script>

<style scoped>

</style>
