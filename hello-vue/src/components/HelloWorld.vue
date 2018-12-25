<template>
  <div id="app">
    <span>firstItem:</span> 
    <input v-model="firstItem" autofocus="autofocus">
    <span>secondItem:</span>
    <input v-model="secondItem">

    <h1 v-text="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="(item, index) in items" v-bind:class="[liClass,{finished: item.isFinish}]" v-on:click="toogleFinish(item)">{{index}}: {{item.label}}</li>      
    </ul>

    <p>child tells me:{{ sonMsg }}</p>
    <a-hello-world msgFromFather='it`s your father`s message' @childTelMe="listenBySon"></a-hello-world>
  </div>
</template>

<script>
import AHelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  /*数据定义*/
  data(){
    return {
      title: 'this is a todo list',
      items: [
        {
          label: 'coding',
          isFinish: false
        },
        {
          label: 'walking',
          isFinish: true
        }
      ],
      liClass: 'isLiClass',
      //newItem: '',
      firstItem: '',
      secondItem: '',
      sonMsg: ''
    }
  },
  /*监听数据变化就会进入*/
  watch: {
    items: {
      handler(items){
        console.log(items);
      },
      deep: true
    }
  },
  /*多个值的变化会影响到一个值用计算属性*/
  computed: {
    newItem: {
      get(){
        alert('get');// 页面加载执行get
        return this.firstItem + this.secondItem;
      },
      set(newVel){
        alert('set');// 数据变化执行set
        this.firstItem = '';
        this.secondItem = '';
      }
    }
  },
  /*方法入口*/
  methods: {
    toogleFinish(item){
      item.isFinish = !item.isFinish;
    },
    addNew(){
      this.items.push(
        {
          label: this.newItem,
          isFinish: false
        }
      );
      this.newItem = '';
    },
    listenBySon(msg){
     this.sonMsg = msg;
    }
  },
  components: {
    AHelloWorld
  }
}
</script>

<style>
.isLiClass {
  font-size: 40px;
  background: #b5b9bc;
  cursor:pointer;
}
.finished {
  color: #05a705;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
