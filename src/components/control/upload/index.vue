<template>
  <div class="upload-box">
    <i class="el-icon-delete" v-if="imageUrl" @click.stop="handleClear"></i>
    <el-upload
      class="upload-demo"
      action=""
      :show-file-list="config.showFile"
      :http-request="handleUpload"
      :accept="config.accept"
      :multiple="config.multiple"
      :limit="config.limit || 1"
      :on-exceed="handleExceed"
      :before-upload="handleBeforeUpload"
      :on-preview="handlePreview"
      :on-remove="handleRemove"
      :before-remove="handleBeforeRemove"
    >
      <el-button v-if="model === 'button'" size="small" type="primary">点击上传</el-button>

      <div v-if="model === 'card'" class="upload-wrap" :style="[sizeStyle]" :class="{'is-round': isRound}">

        <img v-if="imageUrl" :src="imageUrl" class="avatar" :style="[sizeStyle]" >
        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
      </div>
    </el-upload>
  </div>

</template>

<script>
import { props, mixin } from '@/components/control/basis'

export default {
  name: 'UploadComponent',
  mixins: [mixin],
  props: {
    ...props
  },
  data () {
    return {
      val: '',
      imageUrl: ''
    }
  },
  computed: {
    model () {
      return this.config?.model
    },
    sizeStyle () {
      const width = this.config?.width || '100px'
      const height = this.config?.height || '100px'
      return {
        width,
        height
      }
    },
    isRound () {
      return this.config?.isRound || false
    }
  },
  methods: {
    handleUpload (data) {
      const file = data.file
      console.log('file=>', file)

      const form = new FormData()
      form.append('file', file)
      form.append('filename', file.name)

      const requestData = {
        url: this.url,
        method: this.method,
        headers: {
          'Content-Type': 'multipart/form-data'
        },
        data: form
      }

      this.$axios(requestData).then(response => {
        console.log(response.data)
        const servicePath = response.data.servicePath
        this.imageUrl = servicePath
      }).catch(error => {
        console.log(error)
      })
    },
    handleClear () {
      this.imageUrl = ''
    },
    handleExceed () {
      this.$message.warning(`最多只能上传${this.config.limit}个文件`)
    },
    handleBeforeUpload (file) {
      console.log('---', file)
      const isLt2M = file.size / 1024 / 1024 < this.config.maxSize || 2
      if (!isLt2M) {
        this.$message.error(`上传文件大小不能超过 ${this.config.maxSize || 2}MB!`)
      }
      return isLt2M
    },
    handlePreview (file) {
      console.log('111', file)
    },
    handleRemove () {
      console.log('222')
    },
    handleBeforeRemove () {
      return new Promise((resolve, reject) => {
        this.$confirm('是否删除文件？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          resolve()
        }).catch(() => {
        })
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.upload-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  //width: 100px;
  //height: 100px;

  &:hover{
    border-color: #409EFF;
  }
}

.upload-box{
  display : inline-block;
  position: relative;
}
.el-icon-delete{
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  z-index:2;
}
.is-round{
  border-radius: 50%;
}
</style>
