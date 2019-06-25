<template>
  <div class="mod-config">
      <el-form :inline="true" :model="addPersonForm" @keyup.enter.native="getDataList()">
        <el-form-item label="房东姓名">
          <el-input v-model="addPersonForm.name" placeholder="房东姓名" clearable></el-input>
        </el-form-item>
        <el-form-item label="房东电话">
          <el-input v-model="addPersonForm.phone" placeholder="房东电话" clearable></el-input>
        </el-form-item>
        <el-form-item label="状态">
          <el-select v-model="addPersonForm.status" placeholder="请选择状态"></el-select>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">查询</el-button>
          <el-button type="primary" @click="addOrUpdateHandle()">新增</el-button>
        </el-form-item>
      </el-form>
    <el-table :data="tableData" style="width: 100%;" border v-loading="dataListLoading">
      <el-table-column align="center" header-align="center" prop="title" label="房东">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="departType" label="电话">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="payType" label="所属社区">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="towards" label="地址">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="floorArea" label="网格员">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="bathroomType" label="审核人">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="id" label="审核时间">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="status.title" label="状态">
      </el-table-column>
      <el-table-column align="center" header-align="center" prop="rent" label="星级">
      </el-table-column>
      <el-table-column align="center" header-align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="jump(scope.row)">详情</el-button>
          <el-button v-if="scope.row.status.value == 1" type="text" @click="release(scope.row)">发布</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination 
      @size-change="sizeChangeHandle" 
      @current-change="handleCurrentChange" 
      :current-page="currentPage" 
      :page-size="pageSize" 
      :page-sizes="[10, 20, 50, 100]" 
      layout="total, sizes, prev, pager, next, jumper" 
      :total="totalCount">
    </el-pagination>
    <!-- 新增 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="list"></add-or-update>
  </div>
</template>
<script>
import AddOrUpdate from "./lockinstall-add-or-update";
import { formatDate } from '@/utils/format'
export default {
  data() {
    return {
      dialogFormVisible: false,
      dataListLoading: false,
      addOrUpdateVisible: false,
      // 三级联动表单
      form: {
        value: 3301
      },
      addPersonForm: {
        name: "",
        phone: "",
        IdCard: "",
        houseAdress: "",
        community: "",
        area: "",
        status: ''
      },
      province: [],
      provinceValue: "",
      city: [],
      CityValue: "",
      block: [],
      blockValue: "",
      tableData: [], //table列表
      totalCount: 0,
      currentPage: 1,
      pageSize: 10,
      rules: {
        name: [
          { required: true, message: "请输入姓名", trigger: "blur" },
          { min: 2, max: 5, message: "长度在 2 到 5 个字符", trigger: "blur" }
        ],
        phone: [{ required: true, message: "请输入手机号", trigger: "blur" }],
        IdCard: [
          { required: true, message: "请输入身份证", trigger: "blur" },
          { min: 18, max: 18, message: "长度为 18 位", trigger: "blur" }
        ],
        houseAdress: [
          { required: true, message: "请输入房屋地址", trigger: "blur" }
        ],
        community: [
          { required: true, message: "请选择社区", trigger: "change" }
        ],
        area: [{ required: true, message: "请选择所属小区", trigger: "change" }]
      }
    };
  },
  components: {
    AddOrUpdate
  },//注册组件
  created() {
    var data = {
      pageCount: this.currentPage,
      pageSize: this.pageSize,
      city: this.form
    };
    this.list(data);
    this.area();
  },
  activated () {
    var data = {
      pageCount: this.currentPage,
      pageSize: this.pageSize,
      city: this.form
    };
    this.list(data);
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    // 获取省代码
    area() {
      this.axios({
        url: "pub/area",
        method: "post",
        data: { value: 0 }
      })
      .then(data => {
        if (data.data.resultCode == 1000) {
          this.province = data.data.resultData;
        }
      })
      .catch(function(err) {
        console.log("获取省列表失败");
        console.log(err);
      });
    },
    // 获取市代码
    choseCity(CityValue) {
      this.axios({
        url: "pub/area",
        method: "post",
        data: { value: this.provinceValue }
      })
      .then(data => {
        if (data.data.resultCode == 1000) {
          this.city = data.data.resultData;
        }
      })
      .catch(function(err) {
        console.log("获取市列表失败");
        console.log(err);
      });
    },
    // 获取区代码
    choseBlock() {
      this.axios({
        url: "pub/area",
        method: "post",
        data: { value: this.CityValue }
      })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.block = data.data.resultData;
          }
        })
        .catch(function(err) {
          console.log("获取市列表失败");
          console.log(err);
        });
    },
    // 房屋列表
    list(data) {
      this.dataListLoading = true;
      var _this = this;
      if(data) {
        this.axios({
          url: "house/queryallstatus",
          method: "post",
          data: data
        })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.tableData = data.data.resultData.apartments;
            this.totalCount = Number(data.data.resultData.num);
          }
          this.dataListLoading = false;
        })
        .catch(function(err) {
          console.log("失败");
          this.dataListLoading = false;
        })
      } else {
        var data = {
          pageCount: this.currentPage,
          pageSize: this.pageSize,
          city: this.form
        };
        this.list(data);
      }
    },
    // 翻页
    handleCurrentChange(currentPage) {
      this.currentPage = currentPage;
      var data = {
        pageCount: this.currentPage,
        pageSize: this.pageSize,
        city: this.form
      };
      this.list(data);
    },
    sizeChangeHandle(val) {
      this.pageSize = val;
      var data = {
        pageCount: this.currentPage,
        pageSize: this.pageSize,
        city: this.form
      };
      this.list(data);
    },
    // 查询
    ChaXun() {
      var code = this.provinceValue | this.CityValue | this.blockValue;
      var data = {
        pageCount: 1,
        pageSize: this.pageSize,
        city: { value: code }
      };
      this.list(data);
    },
    // 携带参数跳转房屋详情
    jump(row) { 
      this.$router.push({ path: 'demo-fwxq'})
      this.$cookie.set('houseId', row.id)
    },
    //发布房源
    release(row) {
      const that = this;
      const userId = that.$cookie.get('userId');
      this.axios({
        url: "audit/do",
        method: "post",
        data: { 
          apartmentId: row.id,
          userId: userId }
      })
      .then(res => {
        if (res.data.code == 1000) {
          console.log(res.data)
          var data = {
            pageCount: this.currentPage,
            pageSize: this.pageSize,
            city: this.form
          };
          this.list(data);
          this.$message({
          message: '发布成功了',
          type: 'success'
        });
        } else {
          this.$message.error(res.data.msg);
        }
      })
      .catch(function(err) {
        console.log(err);
      });
    },
    // 新增 / 修改
    addOrUpdateHandle (id) {
      this.addOrUpdateVisible = true
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(id)
      })
    },
  }
};
</script>
<style scoped>
.el-select {
  width: 165px;
}
</style>


