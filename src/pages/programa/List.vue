<template>
    <div>
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
    <el-table :data="programa">
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="name" label="栏目名称"></el-table-column>
        <el-table-column prop="num" label="序号"></el-table-column>
        <el-table-column prop="parentId" label="所属产品"></el-table-column>
        <el-table-column label="操作">
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleHandler(slot.row.id)">删除</a>
                <a href="" @click.prevent="toUpdateHandler(slot.row.ip)">修改</a>
            </template>
        </el-table-column>
    </el-table>
        <el-pagination layout="prev,pager,next" :total="50"></el-pagination>
    <el-dialog
        title="添加栏目信息"
        :visible.sync="visible"
        width="60%">
        <el-form :model="form" label-width="80px">
            <el-form-item label="栏目名称">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="序号">
                <el-input v-model="form.num"></el-input>
            </el-form-item>

        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button @click="click= closeModalHand" size="small">取消
            </el-button>
            <el-button type="primary" @click="submitHandler" size="small">确认</el-button>
        </span>
        </el-dialog>    
    </div>
</template>

<script>
import request from '@/utils/request'
export default {
    methods:{
        loadDate(){
            let url = "http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
                this.programa = response.data;
            })
        },
        submitHandler(){
            let url = "http://localhost:6677/category/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                
            },
            data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeModalHand();
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
            programa:[],
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