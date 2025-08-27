<template>
    <el-container style="height: 800px; border: 1px solid #eee">
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
      <el-button @click="addNote">新增</el-button>&nbsp;&nbsp;&nbsp;
      <el-button @click="delNote">删除</el-button>&nbsp;&nbsp;&nbsp;
      <el-button @click="logOut">退出登录</el-button>&nbsp;&nbsp;&nbsp;
    </el-header>
    
    <el-main>
        <el-input
            v-model="menuItems[index].text"
            type="textarea"
            :rows="15"
            placeholder="请输入内容">
        </el-input>
        <br></br>
        <el-button @click="saveText">保存</el-button>
        <el-button @click="askLLM">大模型总结</el-button>
        <br></br>
        <el-input
            v-model="summary"
            type="textarea"
            :rows="13"
            placeholder="笔记总结">
            
        </el-input>
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
            summary: "",
            userId: 0, 
            index: 0,
            response: null,
            saveRes: null,
            addRes: null,
            delRes: null,
            llmRes: null,
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
            this.summary = '';
        },
        saveText: async function(){
            const pyload = {
              id: this.menuItems[this.index].id,
              text: this.menuItems[this.index].text
            }
            this.saveRes = await http.post("/note/save", pyload);
        },
        addNote: async function(){
            this.addRes = await http.get("/note/add/" + this.userId);
            this.qryData();
        },
        delNote: async function(){
            const delId = this.menuItems[this.index].id;
            this.delRes = await http.delete("/note/del/" + delId);
            this.qryData();
            this.index = 0;
        },
        logOut: function(){
            this.$router.push('/');
        },
        askLLM: async function(){
            this.summary = "正在生成请等待。。。";
            const pyload = {
              text: this.menuItems[this.index].text
            }
            this.llmRes = await http.post("/llm/ask", pyload);
            this.summary = this.llmRes.data;
        }
    },
    created(){
        this.qryData();
        this.userId = localStorage.getItem('userId');
    } 
}
</script>