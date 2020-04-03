<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-tab">
                <li
                    v-for="item in menuTab"
                    :key="item.id"
                    :class="{'current':item.current}"
                    @click="toggleMneu(item)"
                >{{item.txt}}</li>
                <el-form
                    :model="ruleForm"
                    status-icon
                    :rules="rules"
                    class="login-form"
                    size="medium"
                >
                    <el-form-item prop="username" class="item-from">
                        <label class="label">邮箱</label>
                        <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                    </el-form-item>
                    <el-form-item prop="password" class="item-from">
                        <label class="label">密码</label>
                        <el-input
                            type="text"
                            v-model="ruleForm.password"
                            autocomplete="off"
                            maxlength="20"
                            minlength="6"
                        ></el-input>
                    </el-form-item>

                    <el-form-item prop="code" class="item-from">
                        <label class="label">验证码</label>
                        <el-row :gutter="11">
                            <el-col :span="15">
                                <el-input v-model.number="ruleForm.code" maxlength="6"></el-input>
                            </el-col>
                            <el-col :span="9">
                                <el-button type="success">获取验证码</el-button>
                            </el-col>
                        </el-row>
                    </el-form-item>

                    <el-form-item>
                        <el-button
                            type="danger"
                            @click="submitForm('ruleForm')"
                            class="login-btn block"
                        >提交</el-button>
                    </el-form-item>
                </el-form>
            </ul>
        </div>
    </div>
</template>

<script>
import {stripscript,validateEmail,validatePwd,validateVCode} from '@/utils/validate'
export default {
  name: "login",
  data() {
    //验证邮箱
    var validateUsername = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else if (validateEmail(value)) {
        callback(new Error("用户名格式有误"));
      } else {
        callback();
      }
    };

    //验证密码
    var validatePassword = (rule, value, callback) => {
        // console.log(stripscript(value))//调用工具过滤里面的特殊字符
        this.ruleForm.password=stripscript(value)//先进行过滤特殊的字符
        value=this.ruleForm.password
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (validatePwd(value)) {
        callback(new Error("密码必须在6-20位之前的数字和字母"));
      } else {
        callback();
      }
    };


    //验证验证码
    var validateCode = (rule, value, callback) => {
      if (value === "") {
        return callback(new Error("请输入验证码"));
      } else if (validateVCode(value)) {
        return callback(new Error("验证码格式有误"));
      } else {
        callback();
      }
    };


    return {
      menuTab: [
        { txt: "登录", current: true },
        { txt: "注册", current: false }
      ],
      ruleForm: {
        username: "",
        password: "",
        code: ""
      },
      rules: {
        username: [{ validator: validateUsername, trigger: "blur" }],
        password: [{ validator: validatePassword, trigger: "blur" }],
        code: [{ validator: validateCode, trigger: "blur" }]
      }
    };
  },
  methods: {
    toggleMneu(data) {
      console.log(data);
      this.menuTab.forEach(ele => {
        console.log("aaaaaa" + JSON.stringify(ele));
        ele.current = false;
      });
      //高光
      data.current = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
  
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background: #344a5f;
}
.login-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background: rgba(0, 0, 0, 1);
  }
}

.login-form {
  text-align: left;
  margin-top: 29px;
  .label {
    display: block;
    margin-bottom: 3px;
    font-size: 14px;
    color: #fff;
  }
  .item-from {
    margin-bottom: 13px;
  }
  .block {
    width: 100%;
    display: block;
  }
  .login-btn {
    margin-top: 19px;
  }
}
</style>


