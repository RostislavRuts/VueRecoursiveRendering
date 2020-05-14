<template>
  <ul class="todolist">
    <ToDoItem
      :item="item"
      :index="index"
      v-for="(item, index) in items"
      :allitems="items"
      :key="item.id"
      @deleteTask="deleteTask"
      @item-up="moveItemUp"
    />

    <AddTask @add-main-list="addTask" :task="task"/>
  </ul>
</template>

<script>
import ToDoItem from "@/components/ToDoItem";
import AddTask from "@/components/AddTask";
export default {
  components: {
    ToDoItem,
    AddTask
  },
  data() {
    return {
      items: [],
      task: "Task"
    };
  },
  methods: {
    addTask(task) {
      this.items.push(task);
      //console.log(this.items)
    },
    deleteTask(index) {
      this.items.splice(index, 1);
    },
    moveItemUp(index) {
      if (index > 0) {
        function swap(arr, a, b) {
          arr[a] = arr.splice(b, 1, arr[a])[0];
        }
        swap(this.items, index, index - 1);
      }
      else this.items.push(this.items.shift());

      
      console.log(index);
    }
  }
};
</script>


