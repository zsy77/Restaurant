<template>
  <div
    style="
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100%;
    "
  >
    <el-card style="width: 1000px; height: 500px">
      <el-form :model="form" :rules="rules" ref="form" label-width="80px">
        <div class="updateinfo">
          <div class="left">
            <el-upload
              class="avatar-uploader"
              :action="'http://' + serverIp + ':8081/employee/upload'"
              :show-file-list="false"
              :on-success="handleAvatarSuccess"
            >
              <img v-if="form.avatarUrl" :src="form.avatarUrl" class="avatar" />
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
            <el-form-item label="姓名" prop="name">
              <el-tag>{{ form.name }}</el-tag>
            </el-form-item>
            <el-form-item label="性别" prop="sex">
              <el-tag>{{ form.genger }}</el-tag>
            </el-form-item>
            <el-form-item label="年龄" prop="phone">
              <el-tag>{{ form.age }}</el-tag>
            </el-form-item>
            <el-form-item label="就任时间" prop="age">
              <el-tag>{{ formattedDate }}</el-tag>
            </el-form-item>
            <el-form-item label="证件号" prop="idCard">
              <el-tag>{{ form.idCard }}</el-tag>
            </el-form-item>
          </div>
          <div class="right">
            <el-form-item>
              <el-button type="primary" @click="save">确 定</el-button>
            </el-form-item>
            <el-form-item label="员工编号" prop="employeeId">
              <el-tag>{{ form.employeeId }}</el-tag>
            </el-form-item>
            <el-form-item label="所属部门" prop="departmentName">
              <el-tag type="danger">{{ form.departmentName }}</el-tag>
            </el-form-item>
            <el-form-item label="部门职位" prop="positionName">
              <el-tag type="danger">{{ form.positionName }}</el-tag>
            </el-form-item>
            <el-form-item label="所属公司" prop="companyName">
              <el-tag type="danger">{{ form.sucompany }}</el-tag>
            </el-form-item>
            <el-form-item label="密码" prop="password">
              <el-input v-model="form.password"></el-input>
            </el-form-item>
            <el-form-item label="地址" prop="address">
              <el-input v-model="form.address"></el-input>
            </el-form-item>
          </div>
        </div>
      </el-form>
    </el-card>
  </div>
</template>

<script>
import { serverIp } from "../../public/config";
import { dateFormat } from "vuex";
export default {
  name: "PersonalDia",
  data() {
    return {
      dialogVisible: false,
      employee: localStorage.getItem("employee")
        ? JSON.parse(localStorage.getItem("employee"))
        : {},
      form: {},
      serverIp: serverIp,
      rules: {
        password: [
          { required: true, message: "账号密码不能为空", trigger: "blur" },
        ],
      },
    };
  },
  created() {
    this.getEmployee().then((res) => {
      console.log(res);
      this.form = res;
    });
  },
  computed: {
    //格式化时间
    formattedDate() {
      const date = new Date(this.form.hireDate);
      const year = date.getFullYear();
      const month = (date.getMonth() + 1).toString().padStart(2, "0");
      const day = date.getDate().toString().padStart(2, "0");
      return `${year}年${month}月${day}日`;
    },
  },
  methods: {
    async getEmployee() {
      return (await this.request.get("/employee/phone/" + this.employee.phone))
        .data;
    },
    save() {
      this.$refs["form"].validate((valid) => {
        if (valid) {
          // 表单校验合法
          this.request.post("/employee", this.form).then((res) => {
            if (res.code === "200") {
              this.$message.success("保存成功");

              // 触发父级更新User的方法
              this.$emit("refreshUser");

              // 更新浏览器存储的用户信息
              this.getEmployee().then((res) => {
                localStorage.setItem("employee", JSON.stringify(res));
              });
              console.log(res);
            } else {
              this.$message.error("保存失败");
            }
          });
        }
      });
    },
    handleAvatarSuccess(res) {
      this.form.avatarUrl = res.data;
    },
  },
};
</script>

<style scoped>
.el-form-item {
  width: 450px;
  height: 50px;
}
/* .updateinfo {
  height: 350px;
  overflow: auto;
} */
.left {
  /* width: 330px; */
  float: left;
}
.right {
  overflow: hidden;
}
.avatar-uploader {
  text-align: center;
  padding-bottom: 10px;
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 138px;
  height: 126px;
  line-height: 126px;
  text-align: center;
}
.avatar {
  width: 138px;
  height: 126px;
  display: block;
}
</style>

