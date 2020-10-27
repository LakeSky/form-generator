<template>
  <div>
    <el-dialog v-bind="$attrs" @close="onClose" title="登录">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <!--show-password-->
          <el-input v-model="form.password" show-password autocomplete="off"></el-input>
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
import { deepClone } from '@/utils/index'

export default {
  components: {},
  inheritAttrs: false,
  props: ['originResource'],
  data() {
    return {
      resources: null,
      dialogTableVisible: false,
      dialogFormVisible: false,
      form: {
        username: '',
        password: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
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
      this.resources = this.originResource.length ? deepClone(this.originResource) : ['']
    },
    onClose() {
      this.$emit('update:visible', false)
    },
    close() {
      console.log("yy");
      this.$emit('update:visible', false)
    },
    handelConfirm() {
      //1.登录成功，并且要在后台建立账号和对应的类型。所有的数据。
      this.$alert('一段网址', '分享网址', {
        confirmButtonText: '确定',
        callback: action => {
          location.href="https://www.baidu.com/";
        }
      });
      this.$emit('update:visible', false)
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
