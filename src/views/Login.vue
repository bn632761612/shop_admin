<template>
  <el-row type="flex" class="row-bg" justify="center" align="middle">
    <el-col :span="6" :xs="14" :sm="12" :md="10" :lg="8" :xl="6">
      <el-form
        ref="loginForm"
        :model="form"
        :rules="rules"
        class="Formname"
        label-position="top"
        label-width="80px"
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="form.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('loginForm')">登录</el-button>
          <el-button>取消</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

 <script>
import axios from "axios";
export default {
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          {
            min: 3,
            max: 12,
            message: "长度在 3 到 12 个字符",
            trigger: "change"
          }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15个字符",
            trigger: "change"
          }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          axios({
            url: "http://localhost:8888/api/private/v1/login",
            method: "post",
            data: this.form
          }).then(res => {
            if (res.data.meta.status == 200) {
              // console.log(res);

              localStorage.setItem("token", res.data.data.token);
              this.$router.push("/home");
            }
          });
        } else {
          // console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>
<style>
.row-bg {
  background-color: hotpink;
  height: 100%;
}
.Formname {
  background-color: #fff;
  padding: 30px 20px;
  border-radius: 10px;
  min-width: 400px;
}
</style>