<template>
  <div class="pos">
    <el-row>
      <!-- 左侧操作栏 -->
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width: 100%">
              <el-table-column prop="name" label="名称"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column label="操作" width="100%" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delOrderGoods(scope.row)">删除</el-button>
                  <!-- scope.row是组件规定死的东西 -->
                  <el-button type="text" size="small" @click="addOrderGoods(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <small>数量：</small>
              {{ totalNum }}个 &nbsp;&nbsp;&nbsp;&nbsp;
              <small>金额：</small>
              {{ totalPrice }}元
            </div>
            <div class="order-button">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="addAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <!-- 右侧产品栏 -->
      <el-col :span="17">
        <!-- 热卖商品 -->
        <div class="title">热卖商品</div>
        <div class="goods-list">
          <ul>
            <li v-for="goods in hotGoods" :key="goods.id" @click="addOrderGoods(goods)">
              <span>{{ goods.name }}</span>
              <span class="item-price">￥{{ goods.price }}</span>
            </li>
          </ul>
        </div>
        <!-- 商品分类 -->
        <div class="goods-category">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <ul class="goods-detail">
                <li v-for="detail in goodsDetail" :key="detail.id" @click="addOrderGoods(detail)">
                  <span class="detail-img">
                    <img :src="detail.img" width="100%">
                  </span>
                  <span class="detail-name">{{ detail.name }}</span>
                  <span class="detail-price">￥{{ detail.price }}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">小食</el-tab-pane>
            <el-tab-pane label="饮料">饮料</el-tab-pane>
            <el-tab-pane label="套餐">套餐</el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "pos",
  data() {
    return {
      totalPrice: 0,
      totalNum: 0,
      tableData: [],
      hotGoods: [
        {
          id: 1,
          name: "香辣鸡腿堡",
          price: 18
        },
        {
          id: 2,
          name: "田园鸡腿堡",
          price: 15
        },
        {
          id: 3,
          name: "和风汉堡",
          price: 15
        },
        {
          id: 4,
          name: "快乐全家桶",
          price: 80
        },
        {
          id: 5,
          name: "脆皮炸鸡腿",
          price: 10
        },
        {
          id: 6,
          name: "魔法鸡块",
          price: 20
        },
        {
          id: 7,
          name: "可乐大杯",
          price: 10
        },
        {
          id: 8,
          name: "雪顶咖啡",
          price: 18
        },
        {
          id: 9,
          name: "大块鸡米花",
          price: 15
        },
        {
          id: 20,
          name: "香脆鸡柳",
          price: 17
        }
      ],
      goodsDetail: [
        {
          id: 1,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "香辣鸡腿堡",
          price: 18
        },
        {
          id: 2,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "田园鸡腿堡",
          price: 15
        },
        {
          id: 3,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "和风汉堡",
          price: 15
        },
        {
          id: 4,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "快乐全家桶",
          price: 80
        },
        {
          id: 5,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "脆皮炸鸡腿",
          price: 10
        },
        {
          id: 6,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "魔法鸡块",
          price: 20
        },
        {
          id: 7,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "可乐大杯",
          price: 10
        },
        {
          id: 8,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "雪顶咖啡",
          price: 18
        },
        {
          id: 9,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "大块鸡米花",
          price: 15
        },
        {
          id: 20,
          img:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1545908132682&di=8e6af0d5831f98ce8ad43599f5b49e0d&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2Fc83d70cf3bc79f3dc2c5af42b3a1cd11728b299f.jpg",
          name: "香脆鸡柳",
          price: 17
        }
      ]
    };
  },
  mounted() {
    let orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods: {
    // 新增（或改变数量）商品至订单列表
    addOrderGoods(goods) {
      // 判断商品是否已存在
      let isExist = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].id === goods.id) {
          isExist = true;
        }
      }

      // 改变订单列表
      if (isExist) {
        let orderArray = this.tableData.filter(item => item.id === goods.id);
        orderArray[0].count++;
        orderArray[0].price = goods.price * orderArray[0].count;
      } else {
        let newGoods = {
          id: goods.id,
          name: goods.name,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.updateTotal();
    },
    delOrderGoods(goods) {
        this.tableData = this.tableData.filter(item => item.id != goods.id);
        this.updateTotal();
    },
    addAllGoods() {
      this.tableData = [];
      this.totalNum = 0;
      this.totalPrice = 0;
    },
    checkout() {
      // 此处可加入相关业务逻辑
      if (this.totalNum != 0) {
        this.tableData = [];
        this.totalNum = 0;
        this.totalPrice = 0;
        this.$message({
          message: '结帐成功，欢迎下次光临',
          type: 'success'
        });
      } else {
        this.$message.error('请选购商品后再进行结帐')
      }
    },
    // 更新小计
    updateTotal() {
      this.totalPrice = 0;
      this.totalNum = 0;
      // 计算总价/数
      this.tableData.forEach(item => {
        this.totalNum += item.count;
        this.totalPrice += item.price;
      });
    }
  }
};
</script>

<style>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
  height: 100%;
  padding: 10px;
}
.order-button {
  margin-top: 10px;
}

.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
  cursor: pointer;
}
.item-price {
  color: #58b7ff;
}

.goods-category {
  clear: both;
  padding: 10px;
}

/* 商品详细　*/
.goods-detail li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.goods-detail li span {
  display: block;
  float: left;
}
.detail-img {
  width: 40%;
}
.detail-name {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.detail-price {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}

.total {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid;
}
</style>