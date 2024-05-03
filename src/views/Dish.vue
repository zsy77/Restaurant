<template>
  <div>
 <div class="wrapper">
  <!-- //白色区域内容 -->
  <div class="page_content">
    <div class="reflex">
    <div class="input_box" >
      <el-input v-model="search_name" placeholder="请输入优惠劵名称" with></el-input>
    </div>
    <el-button @click="searchCoupon" type="primary">查询优惠卷</el-button>
    <el-button @click="dialogVisible = true"  type="primary">增加优惠卷</el-button>
  </div>
  </div>
 </div> 
 <!-- 弹出表单 -->
 <div>

 
    <el-dialog title="表单弹框" :visible.sync="dialogVisible" width="80%">
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="優惠卷名稱">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="最低使用金額">
          <el-input v-model="form.number"></el-input>
        </el-form-item>
        <el-form-item label="打折額度">
          <el-input v-model="form.discount"></el-input>
        </el-form-item>
        <el-form-item label="生效時間">
          <el-input v-model="form.start_time"></el-input>
        </el-form-item>
        <el-form-item label="截止时间">
          <el-input v-model="form.end_time"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
<!-- 表单展示 -->
<div>
  <el-table
    :data="tableData"
    border
    style="width: 100%">
    <el-table-column
      fixed
      prop="name"
      label="名称"
      width="150">
    </el-table-column>
    <el-table-column
      fixed
      prop="minimum"
      label="最低使用金额"
      width="150">
    </el-table-column>
    <el-table-column
      fixed
      prop="discount"
      label="打折额度"
      width="150">
    </el-table-column>
    <el-table-column
      fixed
      prop="start_time"
      label="开始日期"
      width="150">
    </el-table-column>
    <el-table-column
      fixed
      prop="end_time"
      label="结束日期"
      width="150">
    </el-table-column>
    <el-table-column
      fixed
      label="操作"
      width="100">
      <template slot-scope="scope">
        <el-button @click="handleClick(scope.row)" type="text" size="small">删除</el-button>
        <el-button type="text" size="small">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
</div>
</div> 
 </template>
<script>
export default {
  name: "Dish",
  data(){
    return {
      search_name:null,
      dialogVisible:false,
        form:[{
         minimum:" ",
          discount:"",
          start_time:" ",
          end_time:" ",
          name:""
        }],
        tableData:[{
          minimum:" ",
          discount:"",
          start_time:" ",
          end_time:" ",
          name:" "
        }]
        
  };
  }
   
}
</script>
<style scoped>
.reflex{
  display: flex;
     align-items: center ;
}
.input_box{
  width: 2000px;
    margin-right:20px ;
}
</style>