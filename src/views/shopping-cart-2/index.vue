<template>
  <div class="shopping-cart">
    <table class="table-cart">
      <!-- 购物车的头部 -->
      <thead>
        <tr>
          <th style="text-align: center; width:60px" class="cart-th">
            <input id="checkAll" v-model="allChecked" type="checkbox" class="checkAll" @click="checkAllOrNot()">
          </th>
          <th class="cart-th" style="text-align: center; width:60px">序号</th>
          <th class="cart-th" style="text-align: center; width:60px">名称</th>
          <th class="cart-th" style="text-align: center; width:60px">单价</th>
          <th class="cart-th" style="text-align: center; width:100px">数量</th>
          <th class="cart-th" style="text-align: center; width:80px">总价</th>
          <th class="cart-th" style="text-align: center; width:60px">操作</th>

        </tr>
      </thead>
      <!-- 购物车的商品明细 -->
      <tbody>
        <tr v-for="(item,index) in list" :key="index">
          <td style="text-align: center;" class="cart-td">
            <input v-model="item.checked" type="checkbox" class="checkItem" @click="checkItem(index)">
          </td>
          <td class="cart-td">{{ index+1 }}</td>
          <td class="cart-td">{{ item.name }}</td>
          <td class="cart-td">{{ item.price }}</td>
          <td class="cart-td">
            <!-- 对减少商品数量的按钮进行了判断，当相应商品的数量只剩下一个时，绑定 disabled 样式，让它变成不可用。 -->
            <button :disabled="item.count===1" class="btn" @click="handleReduce(index)">-</button>
            <span style="display:inline-block; width:30px">{{ item.count }}</span>
            <button class="btn" @click="handleAdd(index)">+</button>
          </td>
          <td class="cart-td">{{ item.count*item.price }}</td>
          <td class="cart-td">
            <button class="btns" @click="handleRemove(index)"> 删除 </button>
          </td>

        </tr>
      </tbody>
    </table>
    <!-- 选中商品的总价 -->
    <div id="price">
      共计:¥{{ totalPrice }}
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      // 全选
      allChecked: false,

      // 商品列表
      list: [{
        checked: false,
        id: 1,
        name: 'iPhone11 Pro Max',
        price: 12999,
        count: 1
      },
      {
        checked: false,
        id: 2,
        name: 'iPhone11',
        price: 5999,
        count: 1
      },
      {
        checked: false,
        id: 3,
        name: '小米10 Pro',
        price: 5999,
        count: 1
      }
      ],
      // 选中的商品列表，用于计算总价
      checkList: []
    }
  },
  computed: {
    // 总价
    totalPrice() {
      var total = 0
      for (var i = 0; i < this.checkList.length; i++) {
        var item = this.checkList[i]
        total += item.price * item.count
      }
      return total.toLocaleString()
    }
  },
  methods: {
    // 减少按钮方法
    handleReduce(index) {
      // 商品数量至少为1件
      if (this.list[index].count === 1) {
        return
      }
      this.list[index].count--
    },

    // 增加按钮
    handleAdd(index) {
      this.list[index].count++
    },

    // 删除按钮函数
    handleRemove(index) {
      this.list.splice(index, 1)

      // 获取商品序号
      var id = index + 1
      // 移除实际参与计算的商品
      for (var i = 0; i < this.checkList.length; i++) {
        var item = this.checkList[i]
        if (item.id === id) {
          this.checkList.splice(i, 1)
        }
        if ((this.checkList.length === 0) || (this.list.length === 0)) {
          this.allChecked = false
        }
      }
      for (i = 0; i < this.list.length; i++) {
        if (this.list[i].checked === false) {
          this.allChecked = false
        } else { this.allChecked = true }
      }
    },

    // 全选或全不选
    checkAllOrNot() {
      if (this.allChecked === false) {
        // 全选
        for (var i = 0; i < this.list.length; i++) {
          var item = this.list[i]
          item.checked = true
        }
        this.checkList = this.list.concat() // 全选时把商品列表赋给选中列表数组
      } else {
        // 全不选
        for (i = 0; i < this.list.length; i++) {
          item = this.list[i]
          item.checked = false
        }
        this.checkList.splice(0) // 清空数组
      }
    },

    // 勾选或不勾选
    checkItem(index) {
      if (this.list[index].checked === false) {
        // 勾选，加入已选择列表
        this.checkList.push(this.list[index])
        this.checkAll()
      } else {
        // 不勾选，从已选择列表中去除
        var id = index + 1
        // 去除实际参与计算的商品
        for (var i = 0; i < this.checkList.length; i++) {
          var item = this.checkList[i]
          if (item.id === id) {
            this.checkList.splice(i, 1)
          }
        }
        this.allChecked = false
      }
    },

    // 当所有的商品都已被勾选，勾选全选框
    checkAll() {
      // 如果所有的商品都已被勾选，则勾选全选框
      if (this.checkList.length === this.list.length) {
        this.allChecked = true
      }
    }
  }
}
</script>

<style>
* {
	margin: 0px;
	padding: 0px;
}

.shopping-cart {
	width: 600px;
	height: 200px;
	margin: 100px auto;
	text-align: center;
	cursor: default;
}

#price {
	width: 598px;
	height: 40px;
	border-top: 0;
	line-height: 40px;
	text-align: left;
}

.table-cart {
	width: 600px;
	border: 1px solid #EBEEF5;
  border-collapse: collapse;
  border-spacing: 0;
  empty-cells: show;
}

.cart-th {
	font: bold 14px "微软雅黑";
  color: #606266;
  border: 1px solid #EBEEF5;
  border-left: none;
  letter-spacing: 2px;
  text-transform: uppercase;
  text-align: left;
  padding: 12px 6px 12px 12px;

}

.cart-td {
	border-right: 1px solid #EBEEF5;
  border-bottom: 1px solid #EBEEF5;
  background: #fff;
  font-size:14px;
  padding: 12px 6px 12px 12px;
  color: #606266;
}

.btn {
	width: 20px;
	height: 20px;
	border-radius: 5px;
	border: 1px solid #DCDFE6;
	background: #DCDFE6;
	cursor: pointer;
  color: #606266;
}

.btns {
	width: 40px;
	height: 20px;
	border-radius: 5px;
	border: 1px solid #DCDFE6;
	background: #DCDFE6;
	color: #606266;
	line-height: 20px;
	cursor: pointer;
}
</style>
