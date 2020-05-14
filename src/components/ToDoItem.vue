<template>
  <li class="todoitems">
    <div class="mainitems">
      <h3>{{ item.title }}</h3>
      <div class="control" v-if="!parentItem && allitems.length > 1">
        <button class="control-button" @click="itemUp">Up</button>
        <button class="control-button" @click="itemDown">Down</button>
      </div>
      <button class="remove" v-if="parentItem" @click="deleteSubTask">Delete</button>
      <button class="remove" v-if="!parentItem" @click="deleteTask">&times;</button>
    </div>
    <div class="subitems">
      <ul v-if="item.subtasks && item.subtasks.length > 0">
        <ToDoItem
          v-for="(child, subIndex) in item.subtasks"
          v-bind:item="child"
          :index="subIndex"
          :key="child.id"
          :parentItem="item"
          @deleteSubTask="deleteSubTask"
        />
      </ul>
      <!--      add new subtasks for this child      -->
      <AddTask :subtasks="sub" @add-main-list="addSubTask"/>
    </div>
  </li>
</template>

<script>
import AddTask from "./AddTask";
export default {
  name: "ToDoItem",
  data() {
    return {
      sub: "Subtasks"
    };
  },
  props: {
    item: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    parentItem: {
      required: false
    },
    allitems: {
      type: Array
    }
  },
  components: {
    AddTask
  },
  methods: {
    deleteSubTask() {
      this.parentItem.subtasks.splice(this.index, 1);
    },
    deleteTask() {
      this.$emit("deleteTask", this.index);
    },
    addSubTask(task) {
      this.item.subtasks.push(task);
    },
    itemUp() {
      //console.log(this.index)
      this.$emit("item-up", this.index);
    },
    itemDown() {
      this.$emit("item-down", this.index);
      //console.log(this.index)
    }
  }
};
</script>

<style>
.mainitems {
  display: flex;
  align-items: center;
}
.remove {
  background: red;
  color: #fff;
  border-radius: 50%;
  font-weight: bold;
  margin-left: 30px;
}
.subitems {
  margin: 0 0 15px 15px;
}
.control {
  margin: 10px;
}
.control-button {
  display: block;
  width: 50px;
  background: lime;
  font-weight: bolder;
}
</style>