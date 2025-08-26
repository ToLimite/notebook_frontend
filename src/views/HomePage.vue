<template>
    <el-container style="height: 600px; border: 1px solid #eee">
        <el-aside width="250px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="1">
            <p>笔记列表:</p>
            <el-menu-item
                v-for="item in menuItems"
                :key="1">
            </el-menu-item>
        </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="text-align: left; font-size: 16px">
      <span>小小笔记本</span>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      <el-button>新增</el-button>&nbsp;&nbsp;&nbsp;
      <el-button>删除</el-button>&nbsp;&nbsp;&nbsp;
      <el-button @click="qryData">查询</el-button>
    </el-header>
    
    <el-main>
        <el-input
            v-model="menuItems[index]"
            type="textarea"
            :rows="20"
            placeholder="请输入内容">
        </el-input>
        <el-button>保存</el-button>
    </el-main>
  </el-container>
</el-container>
    
</template>

<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>


<script>
import { http } from '@/services';

export default {
    name: 'HomePage',
    data() {
        return{
            index: 0,
            menuItems: [
            ],
        }
    },
    methods: {
        qryData: function(){
            // userId = localStorage.getItem('userId');
            response = http.get("/note/1");
            console.log(response.data);
            // this.menuItems = response.data;
            
        }
    },
    created(){
        this.qryData();
    }
}
</script>