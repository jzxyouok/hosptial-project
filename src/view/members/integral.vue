<template>
  <div class="integral" >
  <!-- <br> -->
  <p>当前位置：{{msg}}</p>
  <br>
  <el-form :inline="true" :model="formInline" class="demo-form-inline">
    <el-form-item label="会员姓名">
      <el-input  placeholder="请输入姓名"></el-input>
    </el-form-item>
      <el-button type="success" class="el-icon-search"@click="onSubmit"></el-button>
    </el-form-item>
  </el-form>

  <div class="content">
  <el-table  :data="tableData"  style="width: 100%" >
    <el-table-column  prop="username" label="会员姓名"  width="180"></el-table-column>
    <el-table-column  prop="accumulative_integral" label="累计积分"  width="180"></el-table-column>
    <el-table-column  prop="avaliable_integral" label="可用积分"  width="180"></el-table-column>
    <el-table-column label="操作">
          <template scope="scope">
            <el-button size="small" class="font-color" ><i class="fa fa-book" aria-hidden="true"></i>查看流水</el-button>
              <i class="fa fa-exchange font-color" aria-hidden="true"></i>
            <el-button size="small" class="font-color" @click="cash_register(scope.$index, scope.row)"> 兑换登记</el-button>
          </template>
        </el-table-column>
      </el-table>
  </el-table>    
  </div>
  <!-- 点击兑换登记出现的弹窗 -->
  <el-dialog title="积分兑换登记" v-model="dialogFormVisible" size="tiny" >
    <el-form ref="form" :model="form" label-width="120px">
      <div class="left"><i class="fa fa-id-card fa-2x" aria-hidden="true"></i><span class="space">{{formInline.username}}</span></div>
      <div  class="right">可用积分：<span class="content_size">{{formInline.accumulative_integral}}元</span></div>  <br><br><br>
      <el-form-item label="兑换物品：">
      <el-input  v-model="form.exchange_item" ></el-input>
    </el-form-item>
    <el-form-item label="兑换使用积分：">
      <el-input v-model="form.use_integral"  ></el-input>
    </el-form-item>
    <el-form-item label="备注：">
      <el-input  type="textarea" v-model="form.tip" ></el-input>
    </el-form-item>
    <el-form-item class="center">
    <template scope="scope">
      <el-button type="primary" @click="handleSave()" :loading="editLoading">确定</el-button>
      <el-button  @click="dialogFormVisible = false">取消</el-button>
    </template>
    </el-form-item>
    </el-form> 
  </el-dialog>   
  <!-- 分页 -->
  <div class="block">
     <el-pagination
       @size-change="handleSizeChange"
       @current-change="handleCurrentChange"
       :current-page="currentPage1"
       :page-sizes="[10, 20, 30, 40]"
       :page-size="100"
       layout="total, sizes, prev, pager, next, jumper"
       :total="tableData.length">
     </el-pagination>
   </div>
   </div>
   </div>
</template>
<script >

export default {
  name: 'integral',
  data () {
    return {
      msg: '会员>会员积分',
      dialogFormVisible:false,
      editLoading:false,//是否显示修改状态
      currentPage1: 5,
      activeIndex: '1',
      formInline: {
        username:'',
        accumulative_integral:'',
        avaliable_integral:'',
      },
      form : {
        exchange_item:'',
        use_integral:'',
        tip:'',
      },
      tableData: [{
                 username:'小红',
                 accumulative_integral:2356,
                 avaliable_integral:562,
               }, {
                 username:'浩明',
                 accumulative_integral:56,
                 avaliable_integral:5,
               }, {
                 username:'偶记',
                 accumulative_integral:236,
                 avaliable_integral:52,
               }, {
                 username:'费红',
                 accumulative_integral:256,
                 avaliable_integral:52,
               }]
    }
  },
  methods: {
       onSubmit() {
         this.$message('模拟数据，这个方法并不管用哦~');
       },

       cash_register (index, row) {
         this.dialogFormVisible = true;
         this.formInline = Object.assign({}, row);
         this.table_index = index;
       },

       handleSave () {
         this.$confirm('确认提交吗？', '提示', {
           confirmButtonText: '确定',
           cancelButtonText: '取消',
           cancelButtonClass: 'cancel'
         }).then(() => {
          this.editLoading = true;//显示正在修改，圆圈跑起来
          this.tableData[this.table_index] = this.formInline;
          this.tableData.splice(this.table_index, 1, this.formInline);
          this.$message({
             message: "操作成功！",
             type: 'success'
           });
           this.editLoading = false;
           this.dialogFormVisible = false;
           console.log(this.form);
         })
      },
      handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      },

      handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      },
      handleSelect(key, keyPath) {
      console.log(key, keyPath);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
.integral{
  padding: 20px;
}

.integral .content{
 /* width: 95%;*/
  height: 400px;
  background: white;
 /* margin: 0 auto;*/
}
.integral .block{
  text-align: center;
}
.integral .center{
  text-align: center;
}
.integral .font-color{
  color: #50bfff; 
   border:0px solid #50bfff;
}
.integral .left{
  float: left; 
  text-align: center;
  width: 50%;
}
.integral .right{
  float: right; 
  text-align: center;
  width: 50%;

}
.integral .center{
  text-align: center;
}

</style>
