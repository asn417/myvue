<template>
  <div style="padding: 10px">
<!--    功能区域-->
    <div style="margin: 10px 0">
      <el-button type="primary">新增</el-button>
      <el-button type="primary">导入</el-button>
      <el-button type="primary">导出</el-button>
    </div>
<!--    搜索区域-->
    <div style="margin: 10px 0">
      <el-input v-model="search" placeholder="请输入关键字" style="width: 20%"></el-input>
      <el-button type="primary" style="margin-left: 5px">查询</el-button>
    </div>
    <el-table :data="tableData" border stripe style="width: 100%"> <!-- stripe斑马纹 -->
      <el-table-column prop="date" label="日期" sortable /><!-- width="180" 加上width固定长度，不加则自适应 -->
      <el-table-column prop="name" label="姓名"  />
      <el-table-column prop="address" label="地址" />
      <el-table-column prop="operate" label="操作" >
        <template #default="scope">
          <el-button type="text" @click="edit(scope.row)">编辑</el-button>
          <el-popconfirm title="确认删除么?"><!-- 二次确认 -->
            <template #reference>
              <el-button type="text" >删除</el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin: 10px 0">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          v-model:current-page="currentPage"
          :page-sizes="[10, 50, 100]"
          :page-size="100"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  components: {

  },
  data() {
    return {
      search: '',
      currentPage: 1,
      total: 10,
      tableData: [
        {
          date: '2016-05-03',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        },
        {
          date: '2016-05-02',
          name: '李白',
          address: 'No. 189, Grove St, Los Angeles',
        },
        {
          date: '2016-05-04',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        },
        {
          date: '2016-05-01',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        }
      ],
    }
  },
  methods:{
    edit(){

    },
    // 查询用户信息列表
    queryUsers(){
      let _this = this;
      //console.log(this.pageInfo);

      this.formData.queryInfo.pagenum = this.pageInfo.pagenum;
      this.formData.queryInfo.pagesize = this.pageInfo.pagesize;

      this.instance.queryUsers(
          this.$baseUrl,this.formData.queryInfo
      ).then(res => {
        //console.log(res.data);
        if (res.data.code == this.global.SucessRequstCode){
          //如果查询成功
          _this.pageInfo.total = res.data.data.length;
          _this.userInfoList = res.data.data;
        }else{
          alert(res.data.message);
        }
      }).catch(error => {
        alert('查询失败！');
        console.log(error);
      });
    },
    // 每页条数改变
    handleSizeChange(newSize) {
      this.pageInfo.pagesize = newSize;
      this.queryUsers();
    },
    // 当前页码改变
    handleCurrentChange(newPage) {
      this.pageInfo.pagenum = newPage;
      this.queryUsers();
    }
  }
}
</script>
