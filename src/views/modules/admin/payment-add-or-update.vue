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
      label-width="120px"
    >
      <el-form-item label="订单编号" prop="orderId">
        <el-input v-model="dataForm.orderId" placeholder="订单编号"></el-input>
      </el-form-item>
      <!-- <el-form-item label="公司id" prop="companyId">
        <el-input v-model="dataForm.companyId" placeholder="公司id"></el-input>
      </el-form-item>-->
      <el-form-item label="参会人员姓名" prop="attenders">
        <el-input v-model="dataForm.attenders" placeholder="参会人员id"></el-input>
      </el-form-item>
      <el-form-item label="缴费时间" prop="payTime">
        <!-- <el-input v-model="dataForm.payTime" placeholder="缴费时间"></el-input> -->
        <el-date-picker v-model="dataForm.payTime" type="datetime" placeholder="选择日期时间"></el-date-picker>
      </el-form-item>
      <el-form-item label="缴费方式" prop="payType">
        <el-input v-model="dataForm.payType" placeholder="缴费方式"></el-input>
      </el-form-item>
      <el-form-item label="缴费项目" prop="payOption">
        <el-input v-model="dataForm.payOption" placeholder="缴费项目"></el-input>
      </el-form-item>
      <el-form-item label="金额" prop="money">
        <el-input v-model="dataForm.money" placeholder="金额"></el-input>
      </el-form-item>
      <el-form-item label="发票抬头" prop="taxTitle">
        <el-input v-model="dataForm.taxTitle" placeholder="发票抬头"></el-input>
      </el-form-item>
      <el-form-item label="纳税人税号" prop="taxNumber">
        <el-input v-model="dataForm.taxNumber" placeholder="纳税人税号"></el-input>
      </el-form-item>
      <el-form-item label="电话" prop="addrPhone">
        <el-input v-model="dataForm.addrPhone" placeholder="地址 电话"></el-input>
      </el-form-item>
      <el-form-item label="开户行及账号" prop="bankAddrAccount">
        <el-input v-model="dataForm.bankAddrAccount" placeholder="开户行及账号"></el-input>
      </el-form-item>
      <el-form-item label="邮寄地址" prop="taxAddress">
        <el-input v-model="dataForm.taxAddress" placeholder="邮寄地址"></el-input>
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
        orderId: "",
        companyId: "",
        attenders: "",
        payTime: "",
        payType: "",
        payOption: "",
        money: "",
        taxTitle: "",
        taxNumber: "",
        addrPhone: "",
        bankAddrAccount: "",
        taxAddress: "",
        createTime: "",
        isDel: ""
      },
      dataRule: {
        orderId: [
          { required: true, message: "订单编号不能为空", trigger: "blur" }
        ],
        companyId: [
          { required: true, message: "公司id不能为空", trigger: "blur" }
        ],
        attenders: [
          { required: true, message: "参会人员id不能为空", trigger: "blur" }
        ],
        payTime: [
          { required: true, message: "缴费时间不能为空", trigger: "blur" }
        ],
        payType: [
          { required: true, message: "缴费方式不能为空", trigger: "blur" }
        ],
        money: [{ required: true, message: "金额不能为空", trigger: "blur" }]
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
            url: this.$http.adornUrl(`/admin/payment/info/${this.dataForm.id}`),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.orderId = data.payment.orderId;
              this.dataForm.companyId = data.payment.companyId;
              this.dataForm.attenders = data.payment.attenders;
              this.dataForm.payTime = data.payment.payTime;
              this.dataForm.payType = data.payment.payType;
              this.dataForm.payOption = data.payment.payOption;
              this.dataForm.money = data.payment.money;
              this.dataForm.taxTitle = data.payment.taxTitle;
              this.dataForm.taxNumber = data.payment.taxNumber;
              this.dataForm.addrPhone = data.payment.addrPhone;
              this.dataForm.bankAddrAccount = data.payment.bankAddrAccount;
              this.dataForm.taxAddress = data.payment.taxAddress;
              this.dataForm.createTime = data.payment.createTime;
              this.dataForm.isDel = data.payment.isDel;
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
              `/admin/payment/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              orderId: this.dataForm.orderId,
              companyId: this.dataForm.companyId,
              attenders: this.dataForm.attenders,
              payTime: this.dataForm.payTime,
              payType: this.dataForm.payType,
              payOption: this.dataForm.payOption,
              money: this.dataForm.money,
              taxTitle: this.dataForm.taxTitle,
              taxNumber: this.dataForm.taxNumber,
              addrPhone: this.dataForm.addrPhone,
              bankAddrAccount: this.dataForm.bankAddrAccount,
              taxAddress: this.dataForm.taxAddress,
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
