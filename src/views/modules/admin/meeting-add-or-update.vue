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
      label-width="136px"
    >
      <el-form-item label="举办单位id" prop="companyId">
        <el-input v-model="dataForm.companyId" placeholder="举办单位id"></el-input>
      </el-form-item>
      <el-form-item label="会议名称(中文)" prop="nameCn">
        <el-input v-model="dataForm.nameCn" placeholder="会议名称(中文)"></el-input>
      </el-form-item>
      <el-form-item label="会议名称(英文)" prop="nameEn">
        <el-input v-model="dataForm.nameEn" placeholder="会议名称(英文)"></el-input>
      </el-form-item>
      <el-form-item label="举办地点" prop="address">
        <el-input v-model="dataForm.address" placeholder="举办地点"></el-input>
      </el-form-item>
      <el-form-item label="开始时间" prop="startTime">
        <el-date-picker v-model="dataForm.startTime" type="datetime" placeholder="选择日期时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="结束时间" prop="endTime">
        <el-date-picker v-model="dataForm.endTime" type="datetime" placeholder="选择日期时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="官方网址" prop="officeWebsite">
        <el-input placeholder="官方网址" v-model="dataForm.officeWebsite">
          <template slot="prepend">Http://</template>
        </el-input>
      </el-form-item>
      <el-form-item label="会议封面图片" prop="titlePicture">
        <el-upload
          action="https://jsonplaceholder.typicode.com/posts/"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt />
        </el-dialog>
      </el-form-item>
      <el-form-item label="涉及学科" prop="subjects">
        <el-input v-model="dataForm.subjects" placeholder="涉及学科"></el-input>
      </el-form-item>
      <el-form-item label="涉及行业" prop="industries">
        <el-input v-model="dataForm.industries" placeholder="涉及行业"></el-input>
      </el-form-item>
      <el-form-item label="会议负责人" prop="serviceEmp">
        <el-input v-model="dataForm.serviceEmp" placeholder="会议负责人"></el-input>
      </el-form-item>
      <el-form-item label="线上报名截止时间" prop="onlineRegDeadline">
        <el-date-picker v-model="dataForm.onlineRegDeadline" type="datetime" placeholder="线上报名截止时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="现场报名截止时间" prop="onsiteRegDeadline">
        <el-date-picker v-model="dataForm.onsiteRegDeadline" type="datetime" placeholder="现场报名截止时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="投稿截止时间" prop="subDeadline">
        <el-date-picker v-model="dataForm.subDeadline" type="datetime" placeholder="投稿截止时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="投稿范围及要求" prop="subRequirement">
        <el-input v-model="dataForm.subRequirement" placeholder="投稿范围及要求" type="textarea"></el-input>
      </el-form-item>
      <el-form-item label="会议介绍" prop="introduction">
        <el-input v-model="dataForm.introduction" placeholder="会议介绍" type="textarea"></el-input>
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
      dialogImageUrl: "",
      dialogVisible: false,
      dataForm: {
        id: 0,
        companyId: "",
        nameCn: "",
        nameEn: "",
        address: "",
        startTime: "",
        endTime: "",
        officeWebsite: "",
        titlePicture: "",
        subjects: "",
        industries: "",
        serviceEmp: "",
        onlineRegDeadline: "",
        onsiteRegDeadline: "",
        subDeadline: "",
        subRequirement: "",
        introduction: "",
        createTime: "",
        modifyTime: "",
        isCheck: "",
        isDel: ""
      },
      dataRule: {
        companyId: [
          { required: true, message: "举办单位id不能为空", trigger: "blur" }
        ],
        nameCn: [
          { required: true, message: "会议名称(中文)不能为空", trigger: "blur" }
        ],
        nameEn: [
          { required: true, message: "会议名称(英文)不能为空", trigger: "blur" }
        ],
        address: [
          { required: true, message: "举办地点不能为空", trigger: "blur" }
        ],
        startTime: [
          { required: true, message: "开始时间不能为空", trigger: "blur" }
        ],
        endTime: [
          { required: true, message: "结束时间不能为空", trigger: "blur" }
        ],
        officeWebsite: [
          { required: true, message: "官方网址不能为空", trigger: "blur" }
        ],
        titlePicture: [
          {
            required: true,
            message: "会议封面图片地址不能为空",
            trigger: "blur"
          }
        ],
        subjects: [
          { required: true, message: "涉及学科不能为空", trigger: "blur" }
        ],
        industries: [
          { required: true, message: "涉及行业不能为空", trigger: "blur" }
        ],
        serviceEmp: [
          { required: true, message: "会议负责人不能为空", trigger: "blur" }
        ],
        onlineRegDeadline: [
          {
            required: true,
            message: "线上报名截止时间不能为空",
            trigger: "blur"
          }
        ],
        onsiteRegDeadline: [
          {
            required: true,
            message: "现场报名截止时间不能为空",
            trigger: "blur"
          }
        ],
        subDeadline: [
          { required: true, message: "投稿截止时间不能为空", trigger: "blur" }
        ],
        subRequirement: [
          { required: true, message: "投稿范围及要求不能为空", trigger: "blur" }
        ],
        introduction: [
          { required: true, message: "会议介绍不能为空", trigger: "blur" }
        ],
        createTime: [
          { required: true, message: "创建时间不能为空", trigger: "blur" }
        ],
        modifyTime: [
          { required: true, message: "修改时间不能为空", trigger: "blur" }
        ],
        isCheck: [
          {
            required: true,
            message: "是否通过审核 0:未通过 1:通过不能为空",
            trigger: "blur"
          }
        ],
        isDel: [
          {
            required: true,
            message: "是否被删除 状态  0：正常   1：删除不能为空",
            trigger: "blur"
          }
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
            url: this.$http.adornUrl(`/admin/meeting/info/${this.dataForm.id}`),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.companyId = data.meeting.companyId;
              this.dataForm.nameCn = data.meeting.nameCn;
              this.dataForm.nameEn = data.meeting.nameEn;
              this.dataForm.address = data.meeting.address;
              this.dataForm.startTime = data.meeting.startTime;
              this.dataForm.endTime = data.meeting.endTime;
              this.dataForm.officeWebsite = data.meeting.officeWebsite;
              this.dataForm.titlePicture = data.meeting.titlePicture;
              this.dataForm.subjects = data.meeting.subjects;
              this.dataForm.industries = data.meeting.industries;
              this.dataForm.serviceEmp = data.meeting.serviceEmp;
              this.dataForm.onlineRegDeadline = data.meeting.onlineRegDeadline;
              this.dataForm.onsiteRegDeadline = data.meeting.onsiteRegDeadline;
              this.dataForm.subDeadline = data.meeting.subDeadline;
              this.dataForm.subRequirement = data.meeting.subRequirement;
              this.dataForm.introduction = data.meeting.introduction;
              this.dataForm.createTime = data.meeting.createTime;
              this.dataForm.modifyTime = data.meeting.modifyTime;
              this.dataForm.isCheck = data.meeting.isCheck;
              this.dataForm.isDel = data.meeting.isDel;
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
              `/admin/meeting/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              companyId: this.dataForm.companyId,
              nameCn: this.dataForm.nameCn,
              nameEn: this.dataForm.nameEn,
              address: this.dataForm.address,
              startTime: this.dataForm.startTime,
              endTime: this.dataForm.endTime,
              officeWebsite: this.dataForm.officeWebsite,
              titlePicture: this.dataForm.titlePicture,
              subjects: this.dataForm.subjects,
              industries: this.dataForm.industries,
              serviceEmp: this.dataForm.serviceEmp,
              onlineRegDeadline: this.dataForm.onlineRegDeadline,
              onsiteRegDeadline: this.dataForm.onsiteRegDeadline,
              subDeadline: this.dataForm.subDeadline,
              subRequirement: this.dataForm.subRequirement,
              introduction: this.dataForm.introduction,
              createTime: this.dataForm.createTime,
              modifyTime: this.dataForm.modifyTime,
              isCheck: this.dataForm.isCheck,
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
    },
    //图片上传
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    }
  }
};
</script>
