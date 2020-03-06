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
      <el-form-item label="姓名" prop="name">
        <el-input v-model="dataForm.name" placeholder="姓名"></el-input>
      </el-form-item>
      <el-form-item label="所属机构(单位名称、公司名称)" prop="organization">
        <el-input v-model="dataForm.organization" placeholder="所属机构(单位名称、公司名称)"></el-input>
      </el-form-item>
      <el-form-item label="职位" prop="positionId">
        <el-input v-model="dataForm.positionId" placeholder="职位"></el-input>
      </el-form-item>
      <el-form-item label="职称" prop="jobTitle">
        <el-input v-model="dataForm.jobTitle" placeholder="职称"></el-input>
      </el-form-item>
      <el-form-item label="电话" prop="phone">
        <el-input v-model="dataForm.phone" placeholder="电话"></el-input>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="dataForm.email" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item label="办公电话">
        <el-input v-model="dataForm.officephone" placeholder="办公电话"></el-input>
      </el-form-item>

      <el-form-item label="参会意向" prop="intention">
        <!-- <el-input v-model="dataForm.intention" placeholder="参会意向"></el-input> -->
        <el-radio v-model="dataForm.intention" label="1">是</el-radio>
        <el-radio v-model="dataForm.intention" label="2">否</el-radio>
      </el-form-item>
      <el-form-item label="住宿要求">
        <el-input v-model="dataForm.room" placeholder="住宿要求" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="接送要求">
        <el-input v-model="dataForm.transfer" placeholder="接送要求" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="备注">
        <el-input v-model="dataForm.note" placeholder="备注" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="联系电话">
        <el-input v-model="dataForm.account" placeholder="联系电话"></el-input>
      </el-form-item>
      <el-form-item label="联系邮箱">
        <el-input v-model="dataForm.emailLx" placeholder="联系邮箱"></el-input>
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
        memberId: "",
        name: "",
        organization: "",
        positionId: "",
        jobTitle: "",
        phone: "",
        email: "",
        officephone: "",
        typeId: "",
        intention: "1",
        room: "",
        transfer: "",
        servicer: "",
        note: "",
        account: "",
        emailLx: "",
        createTime: "",
        modifyTime: "",
        creater: "",
        modifier: "",
        isDel: ""
      },
      dataRule: {
        name: [{ required: true, message: "姓名不能为空", trigger: "blur" }],
        organization: [
          {
            required: true,
            message: "所属机构(单位名称、公司名称)不能为空",
            trigger: "blur"
          }
        ],
        positionId: [
          { required: true, message: "职位不能为空", trigger: "blur" }
        ],
        jobTitle: [
          { required: true, message: "职称不能为空", trigger: "blur" }
        ],
        phone: [{ required: true, message: "电话不能为空", trigger: "blur" }],
        email: [{ required: true, message: "邮箱不能为空", trigger: "blur" }],
        officephone: [
          { required: true, message: "办公电话不能为空", trigger: "blur" }
        ],
        intention: [
          { required: true, message: "参会意向不能为空", trigger: "blur" }
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
            url: this.$http.adornUrl(
              `/admin/attenders/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.meetingId = data.attenders.meetingId;
              this.dataForm.memberId = data.attenders.memberId;
              this.dataForm.name = data.attenders.name;
              this.dataForm.organization = data.attenders.organization;
              this.dataForm.positionId = data.attenders.positionId;
              this.dataForm.jobTitle = data.attenders.jobTitle;
              this.dataForm.phone = data.attenders.phone;
              this.dataForm.email = data.attenders.email;
              this.dataForm.officephone = data.attenders.officephone;
              this.dataForm.typeId = data.attenders.typeId;
              this.dataForm.intention = data.attenders.intention;
              this.dataForm.room = data.attenders.room;
              this.dataForm.transfer = data.attenders.transfer;
              this.dataForm.servicer = data.attenders.servicer;
              this.dataForm.note = data.attenders.note;
              this.dataForm.account = data.attenders.account;
              this.dataForm.emailLx = data.attenders.emailLx;
              this.dataForm.createTime = data.attenders.createTime;
              this.dataForm.modifyTime = data.attenders.modifyTime;
              this.dataForm.creater = data.attenders.creater;
              this.dataForm.modifier = data.attenders.modifier;
              this.dataForm.isDel = data.attenders.isDel;
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
              `/admin/attenders/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              meetingId: this.dataForm.meetingId,
              memberId: this.dataForm.memberId,
              name: this.dataForm.name,
              organization: this.dataForm.organization,
              positionId: this.dataForm.positionId,
              jobTitle: this.dataForm.jobTitle,
              phone: this.dataForm.phone,
              email: this.dataForm.email,
              officephone: this.dataForm.officephone,
              typeId: this.dataForm.typeId,
              intention: this.dataForm.intention,
              room: this.dataForm.room,
              transfer: this.dataForm.transfer,
              servicer: this.dataForm.servicer,
              note: this.dataForm.note,
              account: this.dataForm.account,
              emailLx: this.dataForm.emailLx,
              createTime: this.dataForm.createTime,
              modifyTime: this.dataForm.modifyTime,
              creater: this.dataForm.creater,
              modifier: this.dataForm.modifier,
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
