<template>
  <div>
    <Header></Header>
    <Content :getdata="getdata"></Content>
    <List :arr="arr" :getData="getData"></List>
  </div>
</template>

<script>
  import axios from 'axios'
  import Header from '../components/Header'
  import Content from '../components/Content'
  import List from '../components/List'

  export default {
    name: "index",
    components: {
      Header,
      Content,
      List
    },
    data() {
      return {
        arr: [{
          title: "吃饭",
          isDone: false
        }]
      }
    },
    methods: {
      getData() {
        axios.get("/api/todo").then(res => {
          this.arr = res.data.data;
          console.log(this.arr)
        })
      }
    },
    mounted() {
      this.getData();
    }
  }
</script>

<style scoped>

</style>
