<template>
  <div class="list">
    <div class="list-btn">
      <el-button type="primary" @click="dialogVisible = true" round>添加按钮</el-button>
    </div>
    <div class="perm-search">
      <el-input placeholder="请输入内容" v-model="searchQuery" @input="handleSearch" class="input-with-select">
        <el-button slot="append" icon="el-icon-search" @click="handleSearch(searchQuery)"></el-button>
      </el-input>
    </div>
    <el-table :data="tableData" style="width: 100%" :border="true">
      <el-table-column prop="id" label="ID">
      </el-table-column>
      <el-table-column label="img">
        <template slot-scope="scope">
          <img :src="scope.row.img" style="max-width: 100px; max-height: 100px;" />
        </template>
      </el-table-column>
      <el-table-column prop="goods_name" label="goods_name">
      </el-table-column>
      <el-table-column prop="stock" label="stock">
      </el-table-column>
      <el-table-column prop="price" label="price">
      </el-table-column>
      <el-table-column prop="status" label="status">
      </el-table-column>
      <el-table-column prop="sold" label="sold" sortable>
      </el-table-column>
      <el-table-column prop="created_at" label="created_at" :formatter="formatCreatedAt">
      </el-table-column>
      <el-table-column prop="updated_at" label="updated_at" :formatter="formatCreatedAt">
      </el-table-column>
      <el-table-column label="操作" width="250px">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.row.id)">编辑</el-button>
          <el-button size="mini" @click="handleView(scope.row.id)">查看</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="block">
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage4"
        :page-sizes="[100, 200, 300, 400]" :page-size="100" layout="total, sizes, prev, pager, next, jumper"
        :total="400">
      </el-pagination>
    </div>
    <!-- 添加 -->
    <el-dialog :visible.sync="dialogVisible" title="添加商品" width="50%">
      <el-form :model="ruleForm" status-icon ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="goods_name" prop="goods_name">
          <el-input v-model="ruleForm.goods_name"></el-input>
        </el-form-item>
        <el-form-item label="stock" prop="stock">
          <el-input v-model="ruleForm.stock"></el-input>
        </el-form-item>
        <el-form-item label="price" prop="price">
          <el-input v-model="ruleForm.price"></el-input>
        </el-form-item>
        <el-form-item label="status" prop="status">
          <el-input v-model="ruleForm.status"></el-input>
        </el-form-item>
        <el-form-item label="sold" prop="sold">
          <el-input v-model="ruleForm.sold"></el-input>
        </el-form-item>
        <el-form-item label="img" prop="img">
          <el-input v-model="ruleForm.img"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="submitForm('ruleForm')">添加数据</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
    <!-- 编辑-->
    <el-dialog :visible.sync="editDialog" title="修改商品" width="50%">
      <el-form :model="editForm" status-icon ref="editForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="ID" prop="id" :disabled="true">
          <el-input v-model="editForm.id"></el-input>
        </el-form-item>
        <el-form-item label="goods_name" prop="goods_name">
          <el-input v-model="editForm.goods_name"></el-input>
        </el-form-item>
        <el-form-item label="stock" prop="stock">
          <el-input v-model="editForm.stock"></el-input>
        </el-form-item>
        <el-form-item label="price" prop="price">
          <el-input v-model="editForm.price"></el-input>
        </el-form-item>
        <el-form-item label="status" prop="status">
          <el-input v-model="editForm.status"></el-input>
        </el-form-item>
        <el-form-item label="sold" prop="sold">
          <el-input v-model="editForm.sold"></el-input>
        </el-form-item>
        <el-form-item label="img" prop="img">
          <el-input v-model="editForm.img"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="editDialog = false">取消</el-button>
          <el-button type="primary" @click="submitEditForm('editForm')">保存</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
    <!-- 查看 -->
    <el-dialog :visible.sync="viewDialog" title="商品详情" width="50%">
      <el-form :model="viewForm" status-icon ref="viewForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="ID" prop="id" :disabled="true">
          <el-input v-model="viewForm.id"></el-input>
        </el-form-item>
        <el-form-item label="img">
          <img :src="viewForm.img" style="max-width: 100px; max-height: 100px;" />
        </el-form-item>
        <el-form-item label="goods_name" prop="goods_name">
          <el-input v-model="viewForm.goods_name"></el-input>
        </el-form-item>
        <el-form-item label="stock" prop="stock">
          <el-input v-model="viewForm.stock"></el-input>
        </el-form-item>
        <el-form-item label="price" prop="price">
          <el-input v-model="viewForm.price"></el-input>
        </el-form-item>
        <el-form-item label="status" prop="status">
          <el-input v-model="viewForm.status"></el-input>
        </el-form-item>
        <el-form-item label="sold" prop="sold">
          <el-input v-model="viewForm.sold"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="viewDialog = false">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
 import axios from 'axios';

export default {
  data() {
    return {
      tableData: [],
      searchQuery: '',  //搜索框内容
      currentPage1: 5,
      currentPage2: 5,
      currentPage3: 5,
      currentPage4: 4,
      dialogVisible: false, // 添加弹窗
      ruleForm: {           // 添加表单
        goods_name: '',
        stock: '',
        price: '',
        status: '',
        sold: '',
        img: '',
      },
      editDialog: false, // 编辑弹窗
      editForm: {         // 编辑表单
        id: '',
        goods_name: '',
        stock: '',
        price: '',
        status: '',
        sold: '',
        img: '',
      },
      viewDialog: false,
      viewForm: {         // 查看表单
        id: '',
        goods_name: '',       
        stock: '',
        price: '',
        status: '',
        sold: '',
        img: '',
      },
    }
  },
  created() {
    this.getData();
    this.fetchAndSortData();
  },
  methods: {
    // 获取分页数据
    async fetchAndSortData(page) { 
      await axios.get(`https://liu.zzgoodqc.cn/goodsx/listByPage?page=${page}`).then(res => {
            console.log(res.data);
       })
    },
    // 获取数据
    async getData() {
      await axios.get('https://liu.zzgoodqc.cn/goodsx/goodslist').then(res => {
        this.tableData = res.data.data;
      })
    },
    // 排序
    
    // 搜索
    async handleSearch(goods_name) {
      await axios.get(`https://liu.zzgoodqc.cn/goodsx/search?goods_name=${goods_name}`).then(res => {
        this.tableData = res.data.data;
      })
    },
    // 查看数据
    handleView(id) {
      this.viewDialog = true;
      axios.get(`https://liu.zzgoodqc.cn/goodsx/detail?id=${id}`).then(res => { 
        this.viewForm = res.data.data;
      })
    },
    //添加数据
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          axios.post('https://liu.zzgoodqc.cn/goodsx/add', this.ruleForm).then(() => {
            this.$message.success('添加成功');
            this.dialogVisible = false;
            this.ruleForm = {
                goods_name: '',
                stock: '',
                price: '',
                status: '',
                sold: '',
                img: '',
            }
            this.getData();
          })
       }
      })
    },
    //编辑数据
    //先获取到当前行的数据，然后将数据赋值给 editForm
    handleEdit(id) {
      this.editDialog = true;
      this.editForm = this.tableData.find(row => row.id === id);
    },
    //提交编辑数据
    submitEditForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          axios.post(`https://liu.zzgoodqc.cn/goodsx/update?id=${this.editForm.id}`, this.editForm).then(() => {
            this.$message.success('编辑成功');
            this.editDialog = false;
            this.editForm = {
                id: '',
                goods_name: '',
                stock: '',
                price: '',
                status: '',
                sold: '',
                img: '',
            }
            this.getData();
          })
        }
      })
    },
    //删除
    async handleDelete(id) {
      if (window.confirm('你确定要删除这个条目吗？')) {
        try {
          await axios.get(`https://liu.zzgoodqc.cn/goodsx/delete?id=${id}`)  // 调用删除 API 函数
          // 从 tableData 中移除已删除的数据项
          this.tableData = this.tableData.filter(row => row.id !== id);
          // 添加一个提示消息告知用户删除成功
          this.$message({
            message: '删除成功',
            type: 'success'
          });
          this.getData();
        } catch (error) {
          console.error('Error deleting data:', error);
          // 显示错误消息给用户
          this.$message.error('删除失败: ' + error.message);
        }
      }
    },
    // 格式化时间戳
    formatCreatedAt(row) {
      // 使用 Date 对象来格式化时间戳
      const date = new Date(row.created_at);
      return `${date.getFullYear()}
      -${date.getMonth() + 1}-
      ${date.getDate()} ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`;
    },
    // 分页
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    tableRowClassName({ rowIndex }) {
      if (rowIndex === 1) {
        return 'warning-row';
      } else if (rowIndex === 3) {
        return 'success-row';
      }
      return '';
    }
  }
}
</script>

<style scoped>
.perm-search{
  float: right;
  width: 200px;
}
.list-btn{
  float: left;
}

.el-table .warning-row {
  background: oldlace;
}

.el-table .success-row {
  background: #f0f9eb;
}
</style>