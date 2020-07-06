<template>
    <div>
        <breadcrumd-nav :items="['首页','用户管理','用户列表']"></breadcrumd-nav>
        <!--卡片-->
        <el-card>
           <el-row :gutter="20">
                <el-col :span="8">
                    <el-input placeholder="请输入内容" class="input-with-select">
                        <el-button slot="append" icon="el-icon-search"></el-button>
                    </el-input>
                </el-col>
                <el-col :span="4">
                    <el-button type="primary">搜索</el-button>
                </el-col>
           </el-row>
            <!--表格-->
            <el-table :data="usersData" style="width: 100%" border>
                <el-table-column label="#" type="index"></el-table-column>
                <el-table-column prop="username" label="姓名"></el-table-column>
                <el-table-column prop="email" label="邮箱"></el-table-column>
                <el-table-column prop="mobile" label="电话"></el-table-column>
                <el-table-column prop="role_name" label="角色"></el-table-column>
                <el-table-column prop="mg_state" label="状态">
                    <template slot-scope="scope">
                        <el-switch
                           v-model="scope.row.mg_state"
                           active-color="#13ce66"
                           >
                        </el-switch>
                    </template>

                </el-table-column>
                <el-table-column prop="date" label="操作" width="200">
                    <template slot-scope="scope">
                        <el-tooltip class="item" effect="dark" :enterable="false" content="编辑" placement="top-start">
                            <el-button type="primary" size="mini" icon="el-icon-edit"></el-button>
                        </el-tooltip>
                        <el-tooltip class="item" effect="dark" :enterable="false" content="删除" placement="top-start">
                            <el-button type="danger" size="mini" icon="el-icon-delete"></el-button>
                        </el-tooltip>
                        <el-tooltip class="item" effect="dark" :enterable="false" content="分配角色" placement="top-start">
                            <el-button type="warning" size="mini" icon="el-icon-setting"></el-button>
                        </el-tooltip>
                    </template>

                </el-table-column>
            </el-table>
<!--            表格-->
            <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="usersParms.pagenum"
                    :page-sizes="[2, 5, 10, 20]"
                    :page-size="100"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="total">
            </el-pagination>
        </el-card>


    </div>
</template>

<script>
    import BreadcrumdNav from "components/BreadcrumdNav";
    import {reqUserList} from "network/api"
  export default {
    name: "Users",
    components:{
      BreadcrumdNav
    },

    data(){
      return {
        usersData: [],//表格的数据
        usersParms:{
          query:'',
          pagenum:1,
          pagesize:2
        }, //用户列表请求参数
        total:0 //总条数
      }
    },
    created(){
        this.getUserList()
    },
    methods:{
      //页面改变时候执行
      handleCurrentChange(page){
        // console.log(page)
        this.usersParms.pagenum = page
        this.getUserList()
      },
      //改变页面的条数执行
      handleSizeChange(size){
        this.usersParms.pagesize = size
        this.getUserList()
      },
      //获取用户列表
      async getUserList(){
        const result = await reqUserList(this.usersParms)
        if (result.meta.status !== 200) return this.$message.error("获取用户列表失败")
        const {users,total} = result.data
        this.total = total
        this.usersData = users


      }
    }
  }
</script>

<style scoped>

</style>
