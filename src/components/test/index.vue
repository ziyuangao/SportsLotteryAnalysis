<template>
  <div class="test">
    <el-row :gutter="20">
      <el-col :span="4" :offset="20" >
        <el-button @click="sendMsg">随机插入</el-button>
      </el-col>
    </el-row>
    <!-- <el-table border ref="multipleTable" v-loading="loading" :data="lists" tooltip-effect="dark" @selection-change="handleSelectionChange" class="diy_table" > -->
    <el-table border ref="multipleTable" v-loading="loading" :data="lists" tooltip-effect="dark"  class="diy_table" >
         <el-table-column type="index" label="序号" width="55"> </el-table-column>
         <!-- <el-table-column type="selection"  width="55"> </el-table-column> -->
         <el-table-column label="A" prop="a" ></el-table-column>
         <el-table-column label="B" prop="b" ></el-table-column>
         <el-table-column label="C" prop="c" ></el-table-column>
         <el-table-column label="D" prop="d" ></el-table-column>
         <el-table-column label="E" prop="e" ></el-table-column>
    </el-table>
    <el-pagination
    class="diy_table"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="pageConfig.skip"
      :page-size="pageConfig.limit"
      :total="total"
      layout="prev, pager, next, jumper,sizes">
    </el-pagination>
  </div>
</template>
<script>
import {post} from '@/actions/axios.js'
export default {
  name: "test",
  data() {
    return {
      loading:false,
      multipleSelection:[],
      lists:[],
      pageConfig:{
        limit:10,
        skip:1
      },
      total:undefined,
      formInline: {
        user: "",
        region: ""
      }
    };
  },
  methods: {
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
        this.pageConfig.skip = val;
        this.getList();
      },
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
        this.pageConfig.limit = val;
        this.getList();
      },
    handleSelectionChange(val){
      this.multipleSelection = val;
    },
    getList(){
      this.loading=true;
      let obj = {...this.pageConfig}
      post('/dailyLife/getList',obj,res=>{
        this.loading = false;
        if(res.code == 200){
          this.lists = res.data
          this.total = res.total
        }else{
          this.$message.error('获取列表数据失败')
        }
      })
    },
    test(){
      // 抽奖
      var arr = [
        {type:'A',Subtype:[
          {name:'复活币',probability:'65'},
          {name:'商城道具',probability:'34'},
          {name:'Lv60~70神器武器,传承装备,宠物蛋',probability:'1'},
        ]},
        {type:'B',Subtype:[
          {name:'药剂类',probability:'98'},
          {name:'材料道具类',probability:'1'},
          {name:'Lv60~75神器,辅助装备,魔法石',probability:'1'},
        ]},
        {type:'C',Subtype:[
          {name:'契约类',probability:'65'},
          {name:'魔盒药剂类',probability:'25'},
          {name:'强化增幅相关道具,装扮兑换券',probability:'1'},
        ]},
      ]
      
    },
    sendMsg(){
      let _this = this;
      post('/dailyLife/insertOne',{},res=>{
        if(res.code == 200){
          this.$message.success('插入成功')
          _this.getList();
        }else{
          this.$message.error('插入失败')
        }
      })
    },
    onSubmit() {
        let obj = {...this.formInline}
        post('/dailyLife/test',obj,res=>{
            console.log(res)
        })
    },
  },
  mounted(){
    // this.getList();
    // let minAge = 1,maxAge = 99;
    // let arr = [{},{}];
    // arr[0].values= Array.from({ length: maxAge-minAge }, (_, index) => index+minAge);
    // arr[0].defaultIndex = 19;
    // arr[1].defaultIndex = 29;
    // arr[1].values= Array.from({ length: maxAge-minAge }, (_, index) => index+minAge);
  }
};
</script>
<style scoped>
.test{
    padding: 20px;
    box-sizing: border-box;
    background: white;
}
.diy_table{
  margin-top: 20px;
}
</style>