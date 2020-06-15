<template>
  <div class="hello">
    <h3>{{ msg }}</h3>
    <p>流程</p>

    1.web回答問卷<br/>
    2.web顯示合約內容（如下<br/>
    3.web顯示收到點數（每一個1點<br/><br/>

    區塊鏈評分評價服務

    請給這個產品評分與評價
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
</style>
