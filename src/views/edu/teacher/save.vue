<template>
  <div class="app-container">
    讲师添加
    <el-form label-width="120px">
      <el-form-item label="讲师名称">
        <el-input v-model="teacher.name"/>
      </el-form-item>
      <el-form-item label="讲师排序">
        <el-input-number v-model="teacher.sort" controls-position="right" :min="0"/>
      </el-form-item>
      <el-form-item label="讲师头衔">
        <el-select v-model="teacher.level" clearable placeholder="请选择">
          <el-option :value="1" label="高级讲师"/>
          <el-option :value="2" label="首席讲师"/>
        </el-select>
      </el-form-item>
      <el-form-item label="讲师资历">
        <el-input v-model="teacher.career"/>
      </el-form-item>
      <el-form-item label="讲师简介">
        <el-input v-model="teacher.intro" :rows="10" type="textarea"/>
      </el-form-item>

      <!-- 讲师头像：TODO -->

      <el-form-item>
        <el-button :disabled="saveBtnDisabled" type="primary" @click="saveOrUpdate">保存</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import teacherApi from '@/api/edu/teacher'
export default {
  data() {
    return {
      teacher: {
        name: '',
        sort: 0,
        level: 1,
        career: '',
        intro: '',
        avatar: ''
      },
      saveBtnDisabled: false //保存按钮是否禁用
    }
  },
  created() {
    if (this.$route.params && this.$route.params.id) {
      //从路径获取id值
      const id = this.$route.params.id
      //调用根据id查询的方法
      this.getInfo(id)
    }
  },
  methods: {
    getInfo(id) {
      teacherApi.getTeacherInfo(id)
        .then(response =>{
          this.teacher = response.data.teacher
        })
    },
    saveOrUpdate() {
      //判断修改还是添加，根据teacher是否有id
      if (!this.teacher.id) {
        //添加
        this.saveTeacher()
      } else {
        //修改
        this.updateTeacher()
      }
      
    },

    //修改讲师方法
    updateTeacher() {
      teacherApi.updateTeacher(this.teacher)
        .then(response => {
          //提示信息
          this.$message({
                type: 'success',
                message: '修改成功!'
              });

          //回到列表页面
          this.$router.push({path:'/teacher/list'})
        })
    },

    //添加讲师的方法
    saveTeacher() {
      teacherApi.addTeacher(this.teacher)
        .then(response => {
          //提示信息
          this.$message({
                type: 'success',
                message: '添加成功!'
              });

          //回到列表页面
          this.$router.push({path:'/teacher/list'})
          
        })
    }
  }
}
</script>