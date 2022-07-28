<template>
  <div>
    <input ref="input_file" type="file" @change="handleInputChange" class="input_file">
    <div class="upload-box">
      <el-button type="primary" @click="selectTheFile">选择文件</el-button>
      <el-button type="danger" @click="handleUpload" :loading="loading">点击上传</el-button>
      <p v-if="files" class="file_info">文件名：{{files.name}}</p>
    </div>
    <img v-if="imgUrl" :src="imgUrl" alt="" width="150" height="150">
  </div>

</template>

<script>
export default {
  data () {
    return {
      files: '',
      imgUrl: '',
      loading: false
    }
  },
  methods: {
    handleInputChange (event) {
      this.files = event.target.files[0]
      console.log(this.files)
    },
    selectTheFile () {
      this.$refs.input_file.click()
    },
    async handleUpload () {
      // 获取上传文件的大小
      const size = this.files.size
      // 限制的文件大小
      const targetSize = 1024 * 1024 * 2
      // 获取文件的类型
      const type = this.files.type
      // 上传的文件不能大于2MB
      if (size > targetSize) {
        this.$message.error('上传的文件不能大于2MB')
        return false
      }
      // 上传的文件只能是png
      if (!/png|jpg|jpeg|gif/i.test(type)) {
        this.$message.error('上传的文件只能是png')
        return false
      }
      // 文件需要将文件添加formData中
      const formData = new FormData()
      formData.append('file', this.files)
      formData.append('filename', this.files.name)
      console.log(formData)
      this.loading = true
      const response = await this.$axios({
        url: '/api/upload_single',
        method: 'POST',
        data: formData
      })
      this.loading = false
      this.imgUrl = response.data.servicePath
      console.log(response)
    }
  }
}
</script>
<style lang="scss" scoped>
.input_file{
  display: none;
}
.upload-box{
  width: 300px;
  height: 100px;
  padding: 20px;
  border: 1px dashed #ddd;
  margin: 20px;
}
.file_info{
  color: #666;
  font-size: 14px;
}
</style>
