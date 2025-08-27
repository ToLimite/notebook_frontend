<template>
    <el-container style="height: 600px; border: 1px solid #eee">
        <el-aside width="250px" style="background-color: rgb(238, 241, 246)">
        <el-menu>
            <p>笔记列表:</p>
            <el-menu-item
                v-for="item in menuItems"
                @click="handleClick(item.index)" :key=item.id >
                {{ item.text }}
            </el-menu-item>
        </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="text-align: left; font-size: 16px">
      <span>小小笔记本</span>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      <el-button>新增</el-button>&nbsp;&nbsp;&nbsp;
      <el-button>删除</el-button>&nbsp;&nbsp;&nbsp;
    </el-header>
    
    <el-main>
        <el-input
            v-model="menuItems[index].text"
            type="textarea"
            :rows="20"
            placeholder="请输入内容">
        </el-input>
        <el-button @click="saveText">保存</el-button>
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
            userId: 0, 
            index: 0,
            response: null,
            saveRes: null,
            menuItems: [
            ],
        }
    },
    methods: {
        qryData: async function(){
            this.userId = localStorage.getItem('userId');
            console.log(this.userId);
            this.response = await http.get("/note/"+this.userId);
            this.menuItems = this.response.data;
        },
        handleClick: function(x){
            this.index = x;
        },
        saveText: async function(){
            const pyload = {
              id: this.menuItems[this.index].id,
              text: this.menuItems[this.index].text
            }
            this.saveRes = await http.post("/note/save", pyload);
        }
    },
    created(){
        this.qryData();
    }
}
</script>