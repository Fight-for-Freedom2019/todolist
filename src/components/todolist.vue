<template>
  <div class="todolist">
    <div class="inner">
      <h1>{{ msg }}</h1>
       <input type="text" v-model="searchValue" @keyup.enter="handleSetData" placeholder="请输入并回车添加list" />
       <div style="margin: 20px 0">
          <input type="checkbox" v-model="checked" @change="handleSelect"><label>全选</label>
          <button @click="handleDeleteMore" style="margin-left: 10px; ">批量删除</button>
       </div>
        <li v-for="(item, index) in lists" :key="item.id">
          <input type="checkbox" :value="item.id"  v-model="inputValue"/>
         <label>{{ item.name }}</label>
         <i style="width:10px;height:10px;float: right;cursor: pointer;" @click="handleDeleteSingle(index)">x</i> 
        </li>
      </ul>
      <p style="text-align: left;">
        <span>共{{lists.length}}项</span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todolist',
  data () {
    return {
      msg: 'a todolist',
      searchValue: '',
      lists: [
        // { id: 1, name: "aaa"},
        // { id: 2, name: "bbb"},
        // { id: 3, name: "ccc"},
        // { id: 4, name: "ddd"},
      ],
      indexs: [],
      inputValue: [],
      count: 0,
      is_checked: false,
    }
  },
  computed: {
    checked: {
      get() {
        return (this.lists.length === this.inputValue.length || this.is_checked) && this.lists.length !=0   
      },
      set(val) {
        this.is_checked = val
        if(!val) this.inputValue= [];
      },
    }
  },
  methods: {
    handleSetData() {
      if(this.searchValue){
        this.lists.push({id: this.count++, name: this.searchValue});
        this.searchValue = "";
      }
    },
    handleDeleteMore() {
      if(this.inputValue && this.inputValue.length == 0) return false; 
      this.lists = this.lists.filter( v => this.inputValue.indexOf(v.id) == -1)
      this.inputValue = [];
      this.is_checked = false;
    },
    handleDeleteSingle(index) {
      this.lists.splice(index, 1)
    },
    handleSelect () {
      if(this.checked) {
        const allChecked = this.lists.map(_=> _.id);
        this.inputValue = allChecked
      }else {
        this.inputValue = [];
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.todolist{
  width: 100%;
  height: 100%;
}
.inner {
  width: 200px;
  height: 500px;
  overflow: auto;
  margin: 0 auto;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
  width: auto;
}
li {
  list-style: none;
  /*margin: 0 10px;*/
  text-align: left;
}
a {
  color: #42b983;
}
</style>
