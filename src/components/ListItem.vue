<template>
  <div class="wrapper-list" v-if="activeTab === 1">
    <div v-if="!tasks.length" class="default-state">
      You currently have <span>0</span> tasks. Add a task to get started!
    </div>
    <div v-else class="item-list">
      <ul class="todo-wrapper">
        <li v-for="(task, index) in tasks" :key="index" class="todo-item">
          <div class="content-wrapper">
            <i
              class="bi bi-check-circle uncompleted"
              @click="checkTask(index)"
            ></i>
            <p>{{ task.name }}</p>
          </div>
          <i @click="removeTask(index)" class="bi bi-trash3 icon-trash"></i>
        </li>
      </ul>
    </div>
  </div>
  <div class="wrapper-list" v-if="activeTab === 2">
    <div v-if="!completedTasks.length" class="default-state">
      You currently have <span>0</span> completed tasks!
    </div>
    <div v-else class="item-list">
      <ul class="todo-wrapper">
        <li
          v-for="(task, index) in completedTasks"
          :key="index"
          class="todo-item"
        >
          <div class="content-wrapper">
            <i
              class="bi bi-arrow-repeat completed"
              @click="restoreTask(index)"
            ></i>
            <p class="name-uncompleted">{{ task.name }}</p>
          </div>
          <i
            @click="removeCompletedTask(index)"
            class="bi bi-trash3 icon-trash"
          ></i>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 0;
  border-bottom: 1px solid #8a8e93;
  box-sizing: border-box;
}

.todo-item .content-wrapper {
  display: flex;
  align-items: center;
}

.todo-item i {
  cursor: pointer;
}

.todo-item i.uncompleted {
  color: #8a8e93;
  transition: 0.3s ease-in-out;
}

.todo-item i.uncompleted:hover {
  color: #00cb00;
}

.todo-item i.completed {
  color: #8a8e93;
  transition: 0.3s ease-in-out;
}

.todo-item i.completed:hover {
  color: #fff;
}

.todo-item p {
  margin: 0;
  padding: 0;
  margin-left: 10px;
}

.todo-item span {
  padding: 0;
  font-size: 10px;
}

.todo-item:first-child {
  padding-top: 0;
}

.default-state span {
  color: #60d8f8;
  text-decoration: underline;
}

.todo-wrapper {
  list-style: none;
  padding: 0;
  padding-right: 10px;
}

.name-uncompleted {
  text-decoration: line-through;
}

.icon-trash {
  cursor: pointer;
  color: #8a8e93;
  transition: 0.3s ease-in-out;
}

.icon-trash:hover {
  color: #ff0000;
}
</style>

<script>
export default {
  props: {
    activeTab: {
      type: Number,
      default: 1,
    },
    tasks: {
      type: Array,
      default: () => [],
    },
    completedTasks: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["checkTask", "restoreTask", "removeTask", "removeCompletedTask"],
  setup(props, { emit }) {
    const checkTask = (index) => {
      emit("checkTask", index);
    };

    const restoreTask = (index) => {
      emit("restoreTask", index);
    };

    const removeTask = (index) => {
      emit("removeTask", index);
    };

    const removeCompletedTask = (index) => {
      emit("removeCompletedTask", index);
    };

    return {
      checkTask,
      restoreTask,
      removeTask,
      removeCompletedTask,
    };
  },
};
</script>
