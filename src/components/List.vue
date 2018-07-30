<template>
  <ul class="lists" v-if="this.currentArr">
    <li v-for="(item) in currentArr" :class="{done:item.isDone}">
      {{item.title}}
      <input type="checkbox" v-model="item.isDone" @change="changeStatus(item.isDone,item._id)">
      <button @click="removeSelf(item._id)">删除</button>
    </li>
  </ul>
</template>

<script>
  import axios from 'axios'

  export default {
    props: {
      arr: {
        type: Array
      },
      getData:{
        type:Function
      }
    },
    data() {
      return {
        currentArr: this.arr
      }
    },
    mounted() {
      // console.log(this.currentArr)
    },
    methods: {
      changeStatus(value,id) {
        let isDone = value?1:0;
        axios.patch(`/api/todo/${id}`,{isDone}).then(res=>{
          console.log(res.data.code);
          if(res.data.code == 200){
            alert("更新成功");
            this.getData();
          }
        })
      },
      removeSelf(id){
        axios.delete(`/api/todo/${id}`).then(res=>{
          if(res.data.code == 200){
            alert("删除成功");
            this.getData();
          }else{
            alert("删除失败")
          }
        })
      }
    },
    watch:{
      arr(val){
        this.currentArr = val;
      }
    }
  }
</script>

<style scoped>
  .lists {
    width: 980px;
    margin: 20px auto 0;
    border-bottom: 1px solid #f1f1f1;
  }

  .lists li {
    line-height: 40px;
    height: 40px;
    color: #333;
    font-size: 16px;

  }

  .lists li.done {
    text-decoration: line-through;
  }
</style>
