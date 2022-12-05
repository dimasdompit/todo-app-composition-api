<template>
  <div class="main-wrapper">
    <div class="main-child">
      <HeaderApp
        :totalTasks="tasks.length"
        @addTask="onAddTask"
        @changeTab="changeActiveTab"
      />
      <main class="todo-list">
        <ListItem
          :activeTab="activeTab"
          :tasks="tasks"
          :completedTasks="completedTasks"
          @checkTask="checkTask"
          @restoreTask="restoreTask"
          @removeTask="removeTask"
          @removeCompletedTask="removeCompletedTask"
        />
      </main>
    </div>
  </div>
</template>

<style scoped>
.main-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.main-child {
  background-color: #393f49;
  color: #fff;
  width: 55%;
  border-radius: 8px;
  box-shadow: 5px 5px 19px 1px rgba(0, 0, 0, 0.44);
  -webkit-box-shadow: 5px 5px 19px 1px rgba(0, 0, 0, 0.44);
  -moz-box-shadow: 5px 5px 19px 1px rgba(0, 0, 0, 0.44);
  padding: 35px;
  box-sizing: border-box;
}

.todo-list {
  height: 400px;
  overflow: auto;
}

::-webkit-scrollbar {
  width: 8px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #60d8f8;
  transition: 0.4s ease-in-out;
}

@media (max-width: 992px) {
  .main-child {
    width: 90%;
  }
}

@media (max-width: 768px) {
  .main-child {
    width: 100%;
    border-radius: 0;
  }
}
</style>

<script>
import { reactive, ref } from "vue";
import HeaderApp from "./components/partials/HeaderApp.vue";
import ListItem from "./components/ListItem.vue";

export default {
  components: { HeaderApp, ListItem },
  setup() {
    const tasks = reactive(
      localStorage.getItem("tasks")
        ? JSON.parse(localStorage.getItem("tasks"))
        : []
    );
    const completedTasks = reactive(
      localStorage.getItem("completedTasks")
        ? JSON.parse(localStorage.getItem("completedTasks"))
        : []
    );
    const activeTab = ref(1);

    const onAddTask = (value) => {
      const newTask = {
        name: value,
        completed: false,
      };
      tasks.unshift(newTask);

      saveToLocalstorage("tasks", tasks);
    };

    const checkTask = (index) => {
      tasks[index].completed = true;

      completedTasks.unshift(tasks[index]);
      tasks.splice(index, 1);

      saveToLocalstorage("tasks", tasks);
      saveToLocalstorage("completedTasks", completedTasks);
    };

    const restoreTask = (index) => {
      completedTasks[index].completed = true;

      tasks.unshift(completedTasks[index]);
      completedTasks.splice(index, 1);

      saveToLocalstorage("tasks", tasks);
      saveToLocalstorage("completedTasks", completedTasks);
    };

    const removeTask = (index) => {
      tasks.splice(index, 1);
      saveToLocalstorage("tasks", tasks);
    };

    const removeCompletedTask = (index) => {
      completedTasks.splice(index, 1);
      saveToLocalstorage("completedTasks", completedTasks);
    };

    const changeActiveTab = (value) => {
      activeTab.value = value;
    };

    const saveToLocalstorage = (name, data) => {
      localStorage.setItem(name, JSON.stringify(data));
    };

    return {
      tasks,
      onAddTask,
      checkTask,
      activeTab,
      changeActiveTab,
      completedTasks,
      restoreTask,
      removeTask,
      removeCompletedTask,
    };
  },
};
</script>
