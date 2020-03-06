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
      label-width="150px"
    >
      <el-form-item label="签到人姓名" prop="attenderId">
        <el-input v-model="dataForm.attenderId" placeholder="签到人姓名"></el-input>
      </el-form-item>
      <el-form-item label="签到状态" prop="status">
        <!-- <el-input v-model="dataForm.status" placeholder="签到状态"></el-input> -->
        <el-radio v-model="dataForm.status" label="1">已签到</el-radio>
        <el-radio v-model="dataForm.status" label="2">未签到</el-radio>
      </el-form-item>
      <el-form-item label="签到地点(主会场/分会场)" prop="branchId">
        <!-- <el-input v-model="dataForm.branchId" placeholder="签到地点(主会场/分会场)"></el-input> -->
        <el-select v-model="dataForm.branchId" placeholder="请选择">
          <!-- <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>-->
        </el-select>
      </el-form-item>
      <el-form-item label="是否完成胸卡打印" prop="badge">
        <el-radio v-model="dataForm.badge" label="1">是</el-radio>
        <el-radio v-model="dataForm.badge" label="2">否</el-radio>
      </el-form-item>

      <!-- <el-form-item label="最后签到时间" prop="lastTime">
        <el-input v-model="dataForm.lastTime" placeholder="最后签到时间"></el-input>
      </el-form-item>-->
      <!-- <el-form-item label="flag" prop="flag">
        <el-input v-model="dataForm.flag" placeholder="flag"></el-input>
      </el-form-item> -->
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
        attenderId: "",
        status: "1",
        branchId: "",
        badge: "1",
        createTime: "",
        lastTime: "",
        flag: "",
        isDel: ""
      },
      dataRule: {
        attenderId: [
          { required: true, message: "签到人姓名不能为空", trigger: "blur" }
        ],
        status: [
          { required: true, message: "签到状态不能为空", trigger: "blur" }
        ],
        branchId: [
          {
            required: true,
            message: "签到地点(主会场/分会场)不能为空",
            trigger: "blur"
          }
        ],
        badge: [
          {
            required: true,
            message: "是否完成胸卡打印不能为空",
            trigger: "blur"
          }
        ],
        flag: [{ required: true, message: "flag不能为空", trigger: "blur" }]
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
            url: this.$http.adornUrl(`/admin/sign/info/${this.dataForm.id}`),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.meetingId = data.sign.meetingId;
              this.dataForm.attenderId = data.sign.attenderId;
              this.dataForm.status = data.sign.status;
              this.dataForm.branchId = data.sign.branchId;
              this.dataForm.badge = data.sign.badge;
              this.dataForm.createTime = data.sign.createTime;
              this.dataForm.lastTime = data.sign.lastTime;
              this.dataForm.flag = data.sign.flag;
              this.dataForm.isDel = data.sign.isDel;
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
              `/admin/sign/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              meetingId: this.dataForm.meetingId,
              attenderId: this.dataForm.attenderId,
              status: this.dataForm.status,
              branchId: this.dataForm.branchId,
              badge: this.dataForm.badge,
              createTime: this.dataForm.createTime,
              lastTime: this.dataForm.lastTime,
              flag: this.dataForm.flag,
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
