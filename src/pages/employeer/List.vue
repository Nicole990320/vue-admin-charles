<template>
    <div>
        <el-button type="info" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
    <el-table :data="employeer">   
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="username" label="用户名"></el-table-column>
        <el-table-column prop="gender" label="性别"></el-table-column>
        <el-table-column width="200" prop="telephone" label="联系方式"></el-table-column>
        <el-table-column prop="realname" label="真实姓名"></el-table-column>
        <el-table-column width="200" prop="idCard" label="身份证号"></el-table-column>
        <el-table-column width="200" prop="bankCard" label="银行卡号"></el-table-column>
        <el-table-column label="操作">  
             <template v-slot="slot">
              <!-- {{slot.row}} -->
              <!-- 双大阔号显示脚本 -->
                <a href ="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                <a href="" @click.prevent="toUpdateHandler(slot.row.ip)">修改</a>
            </template>
        </el-table-column>
    </el-table>
     <el-pagination
    layout="prev, pager, next" :total="50">
  </el-pagination>

<el-dialog
  title="录入员工信息"
  :visible.sync="visible"
  width="60%"
 >
<el-form :model="form" label-width="80px"> 
    <el-form-item label="用户名">
        <el-input v-model="form.username"></el-input>
    </el-form-item>    
    <el-form-item label="密码">
        <el-input type="password" v-model="form.password"></el-input>
    </el-form-item>
        <el-form-item label="姓名">
        <el-input v-model="form.realname"></el-input>
    </el-form-item>
    <el-form-item label="性别">
      <el-radio-group v-model="form.gender">
          <el-radio label="男">男</el-radio>
          <el-radio label="女">女</el-radio>
      </el-radio-group>
    </el-form-item> 
      <el-form-item label="手机号">
        <el-input v-model="form.telephone"></el-input>
    </el-form-item>   
      <el-form-item label="身份证号">
        <el-input v-model="form.idCard"></el-input>
    </el-form-item>
    <el-form-item label="银行卡号">
        <el-input v-model="form.bankCard"></el-input>
    </el-form-item>
</el-form>
  <!-- <span>这是一段信息</span> -->
  <span slot="footer" class="dialog-footer">
    <el-button  @click="click = closeModalHand" size="small">取 消</el-button>
    <!--@表示事件绑定-->
    <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
  </span>
</el-dialog>
<!-- /模态框 -->
     </div>
</template>


<script>
import request from '@/utils/request'
import querystring from 'querystring'//把查询对象转换成字符串，方便传输
export default {
    //用于存放网页中需要调用的方法
    methods:{
      //重载员工数据
      loadDate(){
        let url = "http://localhost:6677/waiter/findAll"
        request.get(url).then((response)=>{
          //this指向外部函数的this
          this.employeer = response.data;
        })
      },
      submitHandler(){
      //this.form 对象 ---字符串--> 后台 {type:'customer',age:12}
      // json字符串 '{"type":"customer","age":12}'
      // request.post(url,this.form)
      // 查询字符串 type=customer&age=12
        //通过request与后台进行交互，并且要携带参数
        let url = "http://localhost:6677/waiter/saveOrUpdate"
        request({
          url,
          method:"POST",
          headers:{
              "Content-Type":"application/x-www-form-urlencoded"

          },
          data:querystring.stringify(this.form)
        }).then((response)=>{
          //模态框关闭
          this.closeModalHand();
          //提示消息
          this.$message({
            type:"success",
            message:response.message
          })
        })
      },
        toDeleteHandler(id){
            //确认
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },
        toAddHandler(){
            this.visible=true;
        },
        closeModalHand(){
            this.visible=false;
        },
        toUpdateHandler(row){
                   this.visible = true;
      
        }
    },
    
   //暴露接口
    data(){
        //用于存放要向网页中存放的数据
        return{
            visible:false,
            employeer:[],
            form:{ type:"waiter"}
           
        }
    },
     created(){
         // this为当前vue实例对象
    // vue实例创建完毕 ，在页面加载出来的时候加载数据
    this.loadDate()

}
}
</script>
<style scoped>

</style>