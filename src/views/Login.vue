<template>
  <div class="wrapper">
    <div
        style="margin: 200px auto; background-color: #fff; width: 350px; height: 280px; padding: 20px; border-radius: 10px">
      <div style="margin: 20px 0; text-align: center; font-size: 24px"><b>登 录</b></div>
      <el-form :model="employee" :rules="rules" ref="userForm">
        <el-form-item prop="phone">
          <el-input size="medium" prefix-icon="el-icon-user" v-model="employee.phone"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input size="medium" prefix-icon="el-icon-lock" show-password
                    v-model="employee.password"></el-input>
        </el-form-item>
        <el-form-item style="margin: 10px 0; text-align: center">
          <el-button type="primary" size="small" autocomplete="off" @click="login">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import {setRoutes} from "@/router";

export default {
  name: "Login",
  data() {
    return {
      employee: {},
      rules: {
        phone: [
          {required: true, message: '请输入账号/电话号码', trigger: 'blur'},
          {min: 11, max: 11, message: '输入正确的点话号码', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur'}
        ],
      }
    }
  },
  methods: {
    login() {
      this.$refs['userForm'].validate((valid) => {
        if (valid) {  // 表单校验合法
          this.request.post("/employee/login", this.employee).then(res => {
            if (res.code === '200') {
              localStorage.setItem("employee", JSON.stringify(res.data.employee))  // 存储用户信息到浏览器
              localStorage.setItem("menus", JSON.stringify(res.data.menus))  // 存储用户信息到浏览器
              // 动态设置当前用户的路由
              setRoutes()
              this.$message.success("登录成功")

              // if (res.data.positionId === 'xxxxx') {         //此处可以给需要跳转服务员的登录打卡页面写或者其他员工类自己判断跳转,直接管理的话就是跳转默认地址
              //   this.$router.push("/path")
              // } else {
              //   this.$router.push("/")
              // }
              this.$router.push("/")
            } else {
              this.$message.error(res.msg)
            }
          })
        }
      });
    }
  }
}
</script>

<style scoped>
.wrapper {
  height: 100vh;
  background-image: linear-gradient(to bottom right, #FC466B, #3F5EFB);
  overflow: hidden;
}
</style>
