<template>
  <div>
    <el-upload
      class="upload-demo"
      action="#"
      :on-change="handleFileUpload"
      :multiple="true"
      :auto-upload="false"
      :before-upload="beforeUpload"
      :accept="fileAccept"
      :list-type="'none'"
    >
      <el-button size="small" type="primary">点击上传</el-button>
    </el-upload>
    <el-table :data="fileList" style="width: 100%">
      <el-table-column prop="name" label="文件名" width="180"></el-table-column>
      <el-table-column prop="modificationDate" label="修改日期"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button @click="removeFile(scope.$index)" type="danger" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import moment from'moment';

export default {
  data() {
    return {
      fileList: [],
      fileAccept: ".pdf,.docx,.xlsx"
    };
  },
  methods: {
    beforeUpload(file) {
      const allowedExtensions = this.fileAccept.split(",");
      const fileExtension = file.name.slice(file.name.lastIndexOf(".")).toLowerCase();
      if (!allowedExtensions.includes(fileExtension)) {
        this.$message.error("请上传 PDF、DOCX 或 XLSX 格式的文件");
        return false;
      }
      return true;
    },
    handleFileUpload(fileList) {
      if (fileList && fileList.name) {
        const modificationDate = moment(fileList.raw.lastModified).format('YYYY-MM-DD HH:mm:ss');
        this.fileList.push({
          name: fileList.name,
          modificationDate: modificationDate
        });
      } else {
        console.error('fileList format is not supported:', fileList);
      }
    },
    removeFile(index) {
      this.fileList.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.upload-demo {
  margin-bottom: 20px;
}
/* 使用深度选择器隐藏 el-upload-list */
::v-deep.el-upload-list {
  display: none;
}
</style>