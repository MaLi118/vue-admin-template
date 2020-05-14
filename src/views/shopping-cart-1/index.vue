<template>
  <div class="cart">
    <div class="group-btn">
      <el-button type="success" class="g-btn" @click="addDialog">添加</el-button>
    </div>
    <el-table
      :data="tableData"
      border
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="60" align="center" />
      <el-table-column type="index" label="序号" width="60" align="center" />
      <el-table-column prop="name" label="名称" width="120" align="center" />
      <el-table-column prop="price" label="单价" width="60" align="center" />
      <el-table-column label="数量" width="210" align="center">
        <template slot-scope="scope">
          <el-input-number v-model="scope.row.number" :min="1" :max="100" />
        </template>
      </el-table-column>
      <el-table-column label="总价" width="60" align="center">
        <template scope="scope">
          <div>{{ scope.row.price*scope.row.number }}</div>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="80" align="center">
        <template slot-scope="scope">
          <el-button size="mini" type="danger" align="center" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="total-warpper">
      <h3>共计：¥{{ totalPrice }}</h3>
      <h3>总数：{{ totalNumber }}</h3>
    </div>
    <!-- 添加弹窗的内容 -->
    <el-dialog title="添加信息" :visible.sync="addIsDialog" class="addDialog">
      <el-form :model="formData">
        <el-form-item label="名称" :label-width="formLabelWidth">
          <el-input v-model="formData.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="单价" :label-width="formLabelWidth">
          <el-input v-model="formData.price" autocomplete="off" />
        </el-form-item>
        <el-form-item label="数量" :label-width="formLabelWidth">
          <el-input v-model="formData.number" autocomplete="off" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addIsDialog = false">取 消</el-button>
        <el-button type="primary" @click="addSureBtn">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      addIsDialog: false, // 添加弹窗的初始状态为不显示
      multipleSelection: [], // 存放已选择的数据
      formData: {
        name: '',
        price: '',
        number: ''
      },
      formLabelWidth: '200',
      // 表格中的数据
      tableData: [
        {
          name: 'iPhone 11 Pro',
          price: 8699,
          number: 1
        },
        {
          name: 'HUAWEI P40 Pro 5G',
          price: 7388,
          number: 1
        },
        {
          name: '小米10 Pro',
          price: 5999,
          number: 1
        }
      ]
    }
  },
  // 通过computed计算属性及时改变
  computed: {
    // 总价
    totalPrice() {
      var price_total = 0
      for (var i = 0; i < this.multipleSelection.length; i++) {
        price_total +=
          this.multipleSelection[i].price * this.multipleSelection[i].number
      }
      return price_total.toLocaleString()
    },
    // 总数
    totalNumber() {
      var number_total = 0
      for (var i = 0; i < this.multipleSelection.length; i++) {
        number_total += this.multipleSelection[i].number
      }
      return number_total
    }
  },
  methods: {
    // 点击发生的变化
    handleSelectionChange(val) {
      this.multipleSelection = val // 给定义的数组赋值
    },
    handleDelete(index, row) {
      this.tableData.splice(index, 1) // 删除表格的数据
    },
    // 点击弹出添加弹窗
    addDialog() {
      this.addIsDialog = true
    },
    // 点击添加弹窗的确定按钮
    addSureBtn() {
      this.addIsDialog = false
      if (((this.formData.name !== '') && (typeof (this.formData.name) === 'string')) && ((this.formData.price !== '') && (isNaN(this.formData.price) === false)) && ((this.formData.number !== '') && (isNaN(this.formData.number) === false))) {
        this.tableData.push(this.formData)
      } else { alert('请输入正确的添加信息!不能为空。') }
      // 初始化添加表单
      this.formData = {
        name: '',
        price: '',
        number: ''
      }
    }

  }
}
</script>

<style scoped>
.cart {
  margin: 50px auto;
  width: 651px;
}
.group-btn {
  margin-bottom: 30px;
}

.addDialog{
  width: 651px;
  position: fixed;
  top: 275px;
  left: 255px;
  overflow: auto;
  margin: 0 auto;
}

.el-input {
  width: 244px;
}

.total-warpper {
  text-align: left;
  margin-top: 30px;
}
h3 {
  margin-top: 10px;
}

.g-btn{
  background-color: #409EFF;
  border-color: #409EFF;
}

.el-button--mini{
  height: 38px;
  color: #606266;
  background-color: #DCDFE6;
  border-color: #DCDFE6;
}

</style>
