<template>
  <div class="fm-uplaod-container"
    :id="uploadId"
  >
    <el-upload
            class="avatar-uploader"
            :action="this.uploadUrl"
            :show-file-list="false"
            :before-upload="handleBeforeUpload"
            :on-success="handleSuccess">
      <img v-if="imageUrl" :src="imageUrl" class="avatar">
      <i v-else class="el-icon-plus avatar-uploader-icon"></i>
    </el-upload>
  </div>
</template>

<script>
import Viewer from 'viewerjs'
require('viewerjs/dist/viewer.css')

export default {
  props: {
    value: {
        type: Array,
        default: () => []
    },
    width: {
      type: Number,
      default: 100
    },
    height: {
      type: Number,
      default: 100
    },
    uploadUrl: {
      type: String,
      default: ''
    },
    multiple: {
      type: Boolean,
      default: true
    },
    length: {
      type: Number,
      default: 9
    }
  },
    created: function(){
      //console.info("uploadUrl created:"+this.uploadUrl+",width:"+this.width+" this.heigth:"+this.height)
  },
    mounted:function(){
        console.info("this.value:"+JSON.stringify(this.value))
        if(this.value && this.value.length>0)
        {
            this.imageUrl = this.value[0].url
        }
        //this.fileList=this.value
    },
  data () {
    return {
      fileList: [],
        imageUrl:'',
      viewer: null,
      uploadId: 'upload_' + new Date().getTime()
    }
  },
  computed: {
    miniWidth () {
      if (this.width > this.height) {
        return this.height
      } else {
        return this.width
      }
    }
  },
  methods: {
      handleBeforeUpload (file) {
          //console.info("file:"+file)
      },
      handleChange (file, fileList) {

      },
      handleSuccess(response, file, fileList){
          console.info("response:"+JSON.stringify(response)+"file:"+JSON.stringify(file)+"fileList:"+JSON.stringify(fileList))
            //console.info("自定义："+JSON.stringify(response))
            if(response.code == 0){
                /*this.fileList.push({
                    url: response.url,
                    status: 'success'
                })*/
                this.imageUrl = response.url
                file.url = response.url
                var files = new Array();
                files.push(fileList[0]);
                console.info("files:"+JSON.stringify(files))
                this.$emit('input', files)
            }

      }
  }
}
</script>

<style lang="scss">
.fm-uplaod-container{
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
}
</style>
