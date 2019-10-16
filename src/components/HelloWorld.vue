<template>
  <div class="list">
    <h1>TO DO LIST</h1>
    <h3>{{ unFinishedTodoLength }} items left</h3>
    <div>
      <input v-model="inputValue" type="text" v-on:keyup.enter="enterFn" />
    </div>
    <div class="btns">
      <span
        v-for="(state, index) in states"
        :key="index"
        @click="toggleFilter(state);addClass(index)"
        :class="{ red: index==current }"
      >
        {{ state }}
      </span>
    </div>
    <ul>
      <li v-for="(item, index) in filteredTodos" :key="index">
        <span
          class="opt"
          v-bind:class="{ finished: item.isFinished }"
          v-on:click="finish(item)"
        ></span>
        <label v-bind:class="{ finished: item.isFinished }">{{
          item.label
        }}</label>
        <span class="del" @click="delFn(item)">删除</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      lists: [
        {
          label: "read books",
          isFinished: false
        },
        {
          label: "eat dinner",
          isFinished: true
        }
      ],
      states: ["all", "finished", "unfinished"],
      inputValue: "",
      filter: "all",
      current:0
    };
  },
  methods: {
    enterFn: function() {
      this.lists.push({
        label: this.inputValue,
        isFinished: false
      });
      this.inputValue = "";
    },
    delFn: function(item) {
      var index = this.lists.indexOf(item);
      this.lists.splice(index, 1);
    },
    finish(item) {
      item.isFinished = !item.isFinished;
    },
    toggleFilter(state) {
      this.filter = state;
    },
    addClass:function(index){ 
    this.current=index;
 }
  },
  computed: {
    unFinishedTodoLength() {
      return this.lists.filter(function(item) {
        return item.isFinished === false;
      }).length;
    },
    filteredTodos() {
      if (this.filter === "all") {
        return this.lists;
      }
      if (this.filter === "finished") {
        return this.lists.filter(function(item) {
          console.log(item);
          return item.isFinished;
        });
      }
      if (this.filter === "unfinished") {
        return this.lists.filter(function(item) {
          return item.isFinished === false;
        });
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.finished {
  text-decoration: line-through;
}
.opt {
  display: inline-block;
  width: 10px;
  height: 10px;
  cursor: pointer;
  border: 1px solid #000;
}
span.finished {
  background: red;
}
.del {
  margin: 5px;
  border: 1px solid #000;
  padding: 0 5px;
}
.red {
  border: 1px solid red;
}
.btns span {
  display: inline-block;
  margin-top: 30px;
  margin-left: 5px;
  padding: 0 5px;
  cursor: pointer;
}
</style>
