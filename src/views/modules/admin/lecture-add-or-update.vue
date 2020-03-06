<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmit()"
      label-width="100px"
    >
      <el-form-item label="题目" prop="topic">
        <el-input v-model="dataForm.topic" placeholder="题目"></el-input>
      </el-form-item>
      <el-form-item label="演讲人姓名" prop="name">
        <!-- <el-input v-model="dataForm.speechTime" placeholder="演讲时间"></el-input> -->
        <el-input v-model="dataForm.name" placeholder="姓名"></el-input>

      </el-form-item>
      <!-- <el-form-item label="所属会场(主会场/分会场)" prop="branchId">
        <el-input v-model="dataForm.branchId" placeholder="所属会场(主会场/分会场)"></el-input>

        <el-select v-model="dataForm.branchId" placeholder="主会场">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>-->
      <el-form-item label="演讲时间" prop="speechTime">
        <!-- <el-input v-model="dataForm.speechTime" placeholder="演讲时间"></el-input> -->
        <el-date-picker v-model="dataForm.speechTime" type="datetime" placeholder="演讲时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="摘要" prop="summary">
        <el-input v-model="dataForm.summary" placeholder="请输入摘要内容" type="textarea"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: 0,
        meetingId: "",
        speaker: "",
        topic: "",
        branchId: "",
        speechTime: "",
        summary: "",
        createTime: "",
        isDel: "",
        name: ""
      },
      dataRule: {
        meetingId: [
          { required: true, message: "会议id不能为空", trigger: "blur" }
        ],
        speaker: [
          { required: true, message: "演讲人id不能为空", trigger: "blur" }
        ],
        topic: [{ required: true, message: "题目不能为空", trigger: "blur" }],
        branchId: [
          {
            required: true,
            message: "所属会场(主会场/分会场)不能为空",
            trigger: "blur"
          }
        ],
        speechTime: [
          { required: true, message: "演讲时间不能为空", trigger: "blur" }
        ],
        summary: [{ required: true, message: "摘要不能为空", trigger: "blur" }],
        name: [{ required: true, message: "姓名不能为空", trigger: "blur" }],
        createTime: [
          { required: true, message: "創建時間不能为空", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    init(id) {
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.$http({
            url: this.$http.adornUrl(`/admin/lecture/info/${this.dataForm.id}`),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.meetingId = data.lecture.meetingId;
              this.dataForm.speaker = data.lecture.speaker;
              this.dataForm.topic = data.lecture.topic;
              this.dataForm.branchId = data.lecture.branchId;
              this.dataForm.speechTime = data.lecture.speechTime;
              this.dataForm.summary = data.lecture.summary;
              this.dataForm.createTime = data.lecture.createTime;
              this.dataForm.isDel = data.lecture.isDel;
            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(
              `/admin/lecture/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              meetingId: this.dataForm.meetingId,
              speaker: this.dataForm.speaker,
              topic: this.dataForm.topic,
              branchId: this.dataForm.branchId,
              speechTime: this.dataForm.speechTime,
              summary: this.dataForm.summary,
              createTime: this.dataForm.createTime,
              isDel: this.dataForm.isDel
            })
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                }
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    }
  }
};
</script>
