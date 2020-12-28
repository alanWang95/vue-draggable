<template>
  <div class="row">
    <div class="col-5">
      <h3>可用组件列表</h3>
      <draggable
        class="dragArea list-group"
        :list="list1"
        :group="{ name: 'comp', pull: 'clone', put: false }"
        @change="log"
      >
        <div class="list-group-item" v-for="element in list1" :key="element.id">{{ element.name }}</div>
      </draggable>
    </div>
    <div class="col-5">
      <h3>组件配置页面展示</h3>
      <draggable
        tag="el-collapse"
        class="dragArea list-group"
        :list="list2"
        group="comp"
        @change="log"
        @end='end'
        :move='move'
      >
        <el-collapse
          class="list-group-item left"
          v-for="(element,index) in list2"
          :key="index"
          v-model="activeNames"
          @change="handleChange"
        >
          <el-collapse-item :name="element.id">
            <template slot="title">
              <span>{{element.name}}</span>
              &nbsp;&nbsp;&nbsp;&nbsp; 底色：#<input type="text" @input="inputBlur(element)" v-model="element.color" placeholder="仅支持以“#”开头的6位十六进制数值">
              例：#CCCCCC
              <i class="el-icon-circle-close" @click.stop="deleteItem(index)"></i>
            </template>
            <div>{{ element.content }}</div>
          </el-collapse-item>
        </el-collapse>
      </draggable>
    </div>


  <button @click="goLife">前往效果页</button>
    
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "clone",
  display: "Clone",
  order: 2,
  components: {
    draggable
  },
  data() {
    return {
      list1: [
        { name: "头部导航", id: 1, content: "内容内容内容。。。。",color:'' },
        { name: "中间菜单", id: 2, content: "内容内容内容。。。。",color:'' },
        { name: "店铺列表", id: 3, content: "内容内容内容。。。。",color:'' },
        { name: "底部footer", id: 4, content: "内容内容内容。。。。",color:'' }
      ],
      list2: [],
      activeNames: [],
      count: 0
    };
  },

  methods: {
    log: function(evt) {
      console.log(evt);
      if (evt.added) {
        this.count += 1;
        const item = evt.added.element;
        const idx = this.list2.findIndex(e => e.id === item.id);
        let temp = JSON.parse(JSON.stringify(this.list2));
        temp[idx].id = this.count;
        this.list2 = temp;
        console.log(this.list2);
      }
    },
     move(evt, originalEvent) {
      console.log(evt , 'move')
      console.log(originalEvent) //鼠标位置
    },
    end(evt) {
      // console.log(evt , 'end....')
      // this.drag = true
      // evt.item //可以知道拖动的本身
      // evt.to    // 可以知道拖动的目标列表
      // evt.from  // 可以知道之前的列表
      // evt.oldIndex  // 可以知道拖动前的位置
      // evt.newIndex  // 可以知道拖动后的位置
    },
    inputBlur(val){
      console.log(val);
      console.log(this.list2);
    },
    handleChange: function() {},
    deleteItem: function(index) {
      this.list2.splice(index, 1);
    },
    goLife(){
      console.log(this.list2);
      window.open('http://192.168.13.130:8081/#/configuration?list='+JSON.stringify(this.list2))
    }
  }
};
</script>
<style>
button{
  position: absolute;
  bottom: 10px;
  left: 50px;
  right: 50px;
  width: 100%;
  height: 100px;
  font-size: 20px;
  font-weight: 800;
}
.row {
  display: flex;
  width: 100%;
}

.col-5 {
  flex: 0 0 40%;
  max-width: 40%;
  margin-right: 10%;
}

.list-group {
  display: flex;
  flex-direction: column;
  padding-left: 0;
  margin-bottom: 0;
  border: 0;
  min-height: 500px;
  border: 5px solid #cccccc;
}

.list-group-item:first-child {
  border-top-left-radius: 0.25rem;
  border-top-right-radius: 0.25rem;
}

.list-group-item {
  position: relative;
  display: block;
  padding: 0.75rem 1.25rem;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid rgba(0, 0, 0, 0.125);
  box-sizing: border-box;
}

.el-collapse-item__header {
  border: 0;
  height: 40px;
  line-height: 40px;
}

.el-collapse-item__wrap {
  border-bottom: 0;
}

.list-group-item {
  cursor: move;
}

h3 {
  font-size: 28px;
  margin-bottom: 20px;
}

.el-icon-circle-close {
  color: #c9a2a2;
  font-size: 20px;
  position: absolute;
  right: 50px;
}

.el-icon-circle-close:hover {
  color: #f40;
}
</style>