<template>
  <div class="hello">
    <el-upload
      class="upload-demo"
      action="http://127.0.0.1:8888/api/private/v1/upload"
      :on-preview="handlePictureCardPreview"
      :on-remove="handleRemove"
      :on-success="handleSuccess"
      :before-upload="beforeUp"
      list-type="picture"
    >
      <el-button size="small" type="primary">点击上传</el-button>
    </el-upload>
    <!-- <el-dialog :visible.sync="dialogVisible">
      <img width="100%" :src="dialogImageUrl" alt="" />
    </el-dialog> -->
    <el-image-viewer
      v-show="showViewer"
      :on-close="closeViewer"
      :url-list="[url]"
      ref="aaa"
    />
    <el-select v-model="value" placeholder="请选择">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value">
      </el-option>
    </el-select>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  components: {
    "el-image-viewer": () =>
      import("element-ui/packages/image/src/image-viewer"),
  },
  data() {
    return {
      dialogImageUrl: "",
      dialogVisible: false,
      // 缩放框
      showViewer: false,
      url: "https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/1cb0358a55614d5f49fa442b1b056cc2.jpg?w=2452&h=920",
              options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        value: ''
    };
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      console.log(file);
      this.showViewer = true;
      this.$nextTick(()=>{
        console.log(this.$refs.aaa);
        this.$refs.aaa.$refs.img[0].attributes[2].nodeValue = "transform: scale(1) rotate(0deg); margin-left: 0px; margin-top: 0px; max-height: 100%; max-width: 80%"
      })
    },
    beforeUp(file) {
      var reader = new FileReader();
      reader.onload = function (event) {
        var txt = event.target.result;
        var img = document.createElement("img");
        img.src = txt;
        img.onload = function () {
          console.log(img.width);
          console.log(img.height);
        };
      };
      reader.readAsDataURL(file);
    },
    handleSuccess(response) {
      console.log(response);
    },
    closeViewer() {
      this.showViewer = false;
    },
  },
};
</script>

<style scoped>
/* .el-image-viewer__btn .el-image-viewer__close {
    color: #FFF;
    opacity: unset;
    top: 50px;
    right: 50px;
    width: 50px;
    height: 50px;
    font-size: 50px;
} */

</style>
