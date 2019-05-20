<template>
  <div class="hello">
    <div>
      <a href="#" @click="goodAdd">添加商品</a>
      <p>name:<input type="text" v-model="goodItem.name"></p>
      <p>desc:<input type="text" v-model="goodItem.desc"></p>
      <p>price:<input type="text" v-model="goodItem.price"></p>
      <p>sum:<input type="text" v-model="goodItem.sum"></p>
    </div>
    <div>
<!--      <a @click="getList">查看列表</a>-->
      <h4>商品列表：</h4>
      <div v-for="(item, index) in goodList" :key="index">
        [name]:{{item.name}} [desc]:{{item.desc}} [price]:${{item.price}} [sum]:{{item.sum}}
        <span class="del" @click="goodDelete(item.id)">X</span>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      goodList:[],
      goodItem:{
        name:'',
        desc:'',
        price:'',
        sum:'',
      }
    }
  },
  mounted(){
    this.getList();
  },
  methods:{
    getList(){
      let _that = this;
      this.axios.get('/goodAll')
        .then(res => {
          console.log(res.data)
          this.goodList = res.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    /**
     *添加商品
     */
    goodAdd(){
      this.axios.post('/goodAdd',{
        params:{
          'name':this.goodItem.name,
          'desc':this.goodItem.desc,
          'price':this.goodItem.price,
          'sum':this.goodItem.sum
        }
      })
        .then(res => {
          this.getList();
          this.goodItem.name = '';
          this.goodItem.desc = '';
          this.goodItem.price = '';
          this.goodItem.sum = '';
        })
    },
    /**
     *删除
     */
    goodDelete(paramId){
      this.axios.get('/goodDel',{params:{id:paramId}})
        .then(res=>{
          this.getList();
        })
    },
    /**
     *修改
     */
    goodupdateShow(item){
      this.alterItem.name = item.name;
      this.alterItem.desc = item.desc;
      this.alterItem.price = item.price;
      this.alterItem.sum = item.sum;
    },
    goodupdate(item){
      //this.goodupdateBtn = !this.goodupdateBtn;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
  .del{
    color: red;background: antiquewhite;width: 30px;height: 30px;border-radius: 30px;display: inline-block;text-align: center;line-height: 30px;
  }
.del:hover{
  cursor: pointer;
}
</style>
