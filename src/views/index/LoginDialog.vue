<template>
  <div>
    <el-dialog v-bind="$attrs" @close="onClose" title="">
      <el-form :model="form">
        <el-form-item label="表单名称" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
          <el-tab-pane label="注册" name="register"></el-tab-pane>
          <el-tab-pane label="登录" name="login"></el-tab-pane>
        </el-tabs>
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <el-input v-model="form.password" show-password autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item v-if="activeName=='register'" label="重复密码" :label-width="formLabelWidth">
          <el-input v-model="form.rePassword" show-password autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="close">取 消</el-button>
        <el-button type="primary" @click="handelConfirm">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
  import {deepClone} from '@/utils/index'

  export default {
  components: {},
  inheritAttrs: false,
  props: ['originFormData'],
  data() {
    return {
      activeName: 'register',
      resources: null,
      formData: null,
      dialogTableVisible: false,
      dialogFormVisible: false,
      form: {
        name: '',
        username: '',
        password: '',
        rePassword: ''
      },
      formLabelWidth: '120px'
    }
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {
    onOpen() {
      
    },
    onClose() {
      this.$emit('update:visible', false)
    },
    close() {
      console.log("yy");
      this.$emit('update:visible', false)
    },
    handelConfirm() {
      console.log(this.$attrs.originResource);
      this.$attrs.originResource.name = this.form.name;
      console.log(JSON.stringify(this.$attrs.originResource));
      //1.登录成功，并且要在后台建立账号和对应的类型。所有的数据。
      //2.数据要传给后台建立
      var serverUrl = "http://localhost:8080/";
      var api = "api/auth/register";
      if (this.activeName === 'login') {
        var api = "api/auth/login/back";
      }
      this.$axios.post(serverUrl + api, {//发送请求 跳转页面
        email: this.form.username,
        password: this.form.password,
        rePassword: this.form.rePassword
      }).then((response) => {
        var data = response.data;
        var token = data.data;
        if (data.success) {
          this.$axios.post(serverUrl + "api/myClass/create?token="+ token, {//发送请求 跳转页面
            formJson: JSON.stringify(this.$attrs.originResource)
          }).then((response) => {
            var data = response.data;
            if (data.success) {
              this.$alert('一段网址', '分享网址', {
                confirmButtonText: '确定',
                callback: action => {
                  console.log(data);
                  location.href = serverUrl + "login/tokenGo?page=busi/myClass/home&token=" + token;
                }
              });
            } else {
              this.$message(data.msg);
            }
            console.log(response.data)
          }).catch((response) => {
            console.log("错误" + response)
          });
          this.$emit('update:visible', false)
        }else{
          this.$message(data.msg);
        }
        console.log(response.data)
      }).catch((response) => {
        console.log("错误" + response)
      });
    }
  }
}

</script>
<style lang="scss" scoped>
.add-item{
  margin-top: 8px;
}
.url-item{
  margin-bottom: 12px;
}
</style>
