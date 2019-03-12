<template>
  <div class="fm-uplaod-container"
    :id="uploadId"
  >
    <el-upload
            class="upload-demo"
            :action="this.uploadUrl"
            :before-upload="handleBeforeUpload"
            :on-success="handleSuccess"
            :on-change="handleChange"
            :on-remove="handleRemove"
            :file-list="fileList">
      <el-button size="small" type="primary">点击上传</el-button>
      <!--<div slot="tip" class="el-upload__tip">只能上传文件，且不超过500kb</div>-->
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
      //console.info("uploadUrl created:"+this.uploadUrl)
        //console.info("fileUpload:"+JSON.stringify(this.data))
  },
    mounted:function(){
        //console.info("uploadUrl mounted:"+this.uploadUrl)
        console.info("fileupload this.value:"+JSON.stringify(this.value))
        if(this.value && this.value.length>0){
            this.fileList = this.value
        }

    },

  data () {
    return {
      fileList: [],
      //fileList3: [],
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
      handleRemove(file, fileList) {
          this.fileList = fileList;
      },

      handleChange (file, fileList) {
        //  this.fileList = fileList;
       /*  console.log(this.fileList[0].response)
          this.$nextTick(
              () => {
                  let upload_list_li = document.getElementsByClassName('el-upload-list')[1].getElementsByTagName('a');
                  for (let i = 0; i < upload_list_li.length; i++) {
                      let li_a = upload_list_li[i];
                   //   li_a.href=this.fileList[i].response.url;
                  }
              });*/
      },
      handleSuccess(response, file, fileList){
          //console.info("上传成功："+JSON.stringify(fileList))
          //console.info("response:"+JSON.stringify(response)+"file:"+JSON.stringify(file)+"fileList:"+JSON.stringify(fileList))
            //console.info("自定义："+JSON.stringify(response))
            if(response.code == 0){
                /*fileList.forEach(function(item){

                    if(item.raw){
                        delete item.raw
                    }
                    if(item.response){
                        delete item.response
                    }
                    if(item.percentage){
                        delete item.percentage
                    }
                })*/
               /* this.fileList.push({
                    url: response.url,
                    status: 'success',
                    origName:file.name
                })*/
                //console.info("编辑this.fileList："+JSON.stringify(this.fileList))
                this.$emit('input', fileList)

                let upload_list_li = document.getElementsByClassName('el-upload-list')[1].getElementsByTagName('a');
                for (let i = 0; i < upload_list_li.length; i++) {
                    let li_a = upload_list_li[i];
                       li_a.href=fileList[i].response.url;
                }
            }

      }
  }
}
</script>

<style lang="scss">
.fm-uplaod-container{
  .upload-file{
    margin: 0 10px 10px 0;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    // background: #fff;
    overflow: hidden;
    background-color: #fff;
    border: 1px solid #c0ccda;
    border-radius: 6px;
    box-sizing: border-box;
    position: relative;
    vertical-align: top;

    &:hover{
      .uplaod-action{
        display: flex;
      }
    }

    .uplaod-action{
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0,0,0,0.6);
      display: none;
      justify-content: center;
      align-items: center;

      i{
        color: #fff;
        cursor: pointer;
        margin: 0 5px;
      }
    }

    &.is-success{
      .item-status{
        position: absolute;
        right: -15px;
        top: -6px;
        width: 40px;
        height: 24px;
        background: #13ce66;
        text-align: center;
        transform: rotate(45deg);
        box-shadow: 0 0 1pc 1px rgba(0,0,0,.2);

        &>i{
          font-size: 12px;
          margin-top: 11px;
          color: #fff;
          transform: rotate(-45deg);
        }
      }
    }

    &.uploading{
      &:before{
        display: block;
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0,0,0,0.3);
      }
    }

    .upload-progress{
      position: absolute;

      .el-progress__text{
        color: #fff;
        font-size: 16px !important;
      }
    }

    img{
      max-width: 100%;
      max-height: 100%;
      vertical-align: middle;
    }
  }

  .el-upload--picture-card{
    position: relative;
    overflow: hidden;

    .el-icon-plus{
      position: absolute;
      top: 50%;
      left: 50%;
    }
  }

  .upload-input{
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: block;
    opacity: 0;
    cursor: pointer;
  }
}
</style>
