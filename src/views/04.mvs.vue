<template>
  <div class="mvs-container">
    <div class="filter-wrap">
      <div class="seciton-wrap">
        <span class="section-type">地区:</span>
        <ul class="tabs-wrap">
          <li class="tab">
            <span class="title" :class="{active:area=='全部'}" @click="area='全部'">全部</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:area=='内地'}" @click="area='内地'">内地</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:area=='港台'}" @click="area='港台'">港台</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:area=='欧美'}" @click="area='欧美'">欧美</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:area=='日本'}" @click="area='日本'">日本</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:area=='韩国'}" @click="area='韩国'">韩国</span>
          </li>
        </ul>
      </div>
      <div class="type-wrap">
        <span class="type-type">类型:</span>
        <ul class="tabs-wrap">
          <li class="tab">
            <span class="title" :class="{active:type=='全部'}" @click="type='全部'">全部</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:type=='官方版'}" @click="type='官方版'">官方版</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:type=='原声'}" @click="type='原声'">原声</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:type=='现场版'}" @click="type='现场版'">现场版</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:type=='网易出品'}" @click="type='网易出品'">网易出品</span>
          </li>
        </ul>
      </div>
      <div class="order-wrap">
        <span class="order-type">排序:</span>
        <ul class="tabs-wrap">
          <li class="tab">
            <span class="title" :class="{active:order=='上升最快'}" @click="order='上升最快'">上升最快</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:order=='最热'}" @click="order='最热'">最热</span>
          </li>
          <li class="tab">
            <span class="title" :class="{active:order=='最新'}" @click="order='最新'">最新</span>
          </li>
        </ul>
      </div>
    </div>
    <!-- mv容器 -->
    <!-- 推荐MV -->
    <div class="mvs">
      <div class="items">
        <div class="item" @click="toMv(item.id)" v-for="item in list" :key="item.id">
          <div class="img-wrap">
            <img :src="item.cover" alt="" />
            <div class="num-wrap">
              <div class="iconfont icon-play"></div>
              <!-- 使用全局过滤器处理播放次数 -->
              <div class="num">{{item.playCount | formatCount}}</div>
              <!-- <div class="num">{{item.playCount}}</div> -->
            </div>
          </div>
          <div class="info-wrap">
            <div class="name">{{item.name}}</div>
            <div class="singer">{{item.artistName}}</div>
          </div>
        </div>
      </div>
      <!-- 分页器 -->
      <el-pagination
        @current-change="handleCurrentChange"
        background
        layout="prev, pager, next"
        :total="total"
        :current-page="page"
        :page-size="5"
        :limit="limit"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'mvs',
  data() {
    return {
      // 总条数
      total: 20,
      // 页码
      page: 1,
      // 页容量
      limit: 8,
      // 地区
      area: "全部",
      // 类型
      type:"全部",
      // 排序
      order:"上升最快",
      // 查询结果
      list: []
    };
  },
  watch: {
    area(){
      // 返回第一页
      this.page = 1
      // 获取数据
      this.getList()
    },
    type(){
      // 返回第一页
      this.page = 1
      // 获取数据
      this.getList()
    },
    order(){
      // 返回第一页
      this.page = 1
      // 获取数据
      this.getList()
    },
  },
  created() {
    this.getList()
  },
  
  methods: {
    // 获取列表数据
    getList(){
      axios({
        url:'https://lianghj.top:3000/mv/all',
        method:'get',
        params:{
          area:this.area,
          type:this.type,
          order:this.order,
          limit:this.limit,
          // 偏移值 分页 （页码-1）*数量
          offset:(this.page - 1) * this.limit
        }
      }).then(res=>{
        this.list = res.data.data
        // 处理播放次数大于十万显示xx万。
        // for (let i = 0; i < this.list.length; i++) {
        //   if(this.list[i].playCount > 100000)
        //     this.list[i].playCount = parseInt(this.list[i].playCount / 10000) + '万'
        // }

        // 保存总条数
        // 接口问题：有的其他页没有count，所以要设置有count就赋值
        if(res.data.count)
          this.total = res.data.count
      })
    },
    toMv(id) {
      this.$router.push(`/mv?q=${id}`);
    },
    // 页面改变的回调函数
    handleCurrentChange(val) {
      // 保存页码重新获取数据
      this.page = val
      this.getList()
    }
  }
};
</script>

<style >

</style>
