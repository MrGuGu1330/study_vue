<template>
  <div>
    <!-- 面包屑导航 -->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>演员管理</el-breadcrumb-item>
      <el-breadcrumb-item>新增演员</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 分割线 -->
    <el-divider></el-divider>

    <!-- 表单 -->
    <el-form label-width="80px" style="width: 300px"
      ref="form"
      :rules="rules"
      :model="form"  
    >
      <el-form-item label="姓名" prop="actorName"> 
        <el-input v-model="form.actorName" placeholder="请输入姓名" />
      </el-form-item>
      <el-form-item label="选择头像" prop="actorAvatar">
        <el-upload
            class="avatar-uploader"
            action="http://localhost:9000/upload"
            :on-success="handleAvatarSuccess"
            :show-file-list="false">
            <img v-if="form.actorAvatar" :src="form.actorAvatar" class="avatar">
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submit">立即新增</el-button>
        <el-button>返回</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import myaxios from '@/http/MyAxios';
export default {
  data() {
    return {
      form: {
        actorName: "", // 绑定姓名
        actorAvatar: "", // 绑定上传图片的访问地址
      },
      rules:{
        actorName:[{required:true,message:'姓名必填',trigger:'blur'}],
        actorAvatar:[{required:true,message:'头像必填'}]
      }
    };
  },
  methods: {
    // 上传头像成功后自动调用，将会自动传入响应结果res与文件对象file
    handleAvatarSuccess(res,file){
        console.log(res)
        this.form.actorAvatar = res.data
    },
    submit(){
        // 先验证表单是否符合要求
        this.$refs['form'].validate(valid => {
            if(valid){ //valid返回true,意味着量字段都验证通过
                // 发送post请求，执行添加业务
                console.log('验证通过')
                let url = 'http://localhost:3010/movie-actor/add'
                myaxios.post(url,this.form).them(res => {
                    console.log(res)
                    if(res.data.code == 200){
                        this.$refs['form'].resetFields()
                        this.$message({
                            message:'新增演员完成',
                            type:'success'
                        })
                    }
                })
            }
        })
    }
  },
};
</script>

<style lang="scss">
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
</style>
