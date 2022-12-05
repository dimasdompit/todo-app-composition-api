<template>
  <header>
    <div class="row header-wrapper">
      <div class="col-lg-6">
        <h3 class="title">{{ currentDate }}</h3>
        <span class="subtitle">{{ totalTasks }} Active Tasks</span>
      </div>
      <div class="col-lg-6 mt-2">
        <ul class="menu-wrapper">
          <li class="menu-list" :class="[activeTab == 1 ? 'active' : '']">
            <a @click.prevent="changeTab(1)" href="#">Incomplete Tasks</a>
          </li>
          <li class="menu-list" :class="[activeTab == 2 ? 'active' : '']">
            <a @click.prevent="changeTab(2)" href="#">Completed Tasks</a>
          </li>
        </ul>
      </div>
    </div>
    <div class="row" v-if="activeTab === 1">
      <div class="col-12 input-todo">
        <div class="input-group">
          <input
            v-model="task"
            type="text"
            class="form-control"
            placeholder="Enter a task..."
            aria-label="Enter a task..."
            aria-describedby="button-add"
            @keyup.enter="addTask"
          />
          <button
            class="btn btn-add"
            type="button"
            id="button-add"
            @click="addTask"
          >
            Add Task
          </button>
        </div>
        <small v-if="errors" class="error-message">{{ errors }}</small>
      </div>
    </div>
    <hr class="mt-4" />
  </header>
</template>

<style scoped>
.header-wrapper {
  display: flex;
  align-items: center;
}
.title {
  margin: 0;
  font-size: 17px;
  font-weight: bold;
}
.subtitle {
  color: #60d8f8;
  font-size: 13px;
}
.menu-wrapper {
  display: flex;
  list-style: none;
  justify-content: flex-end;
}
.menu-list {
  margin-right: 35px;
}
.menu-list a {
  text-decoration: none;
  color: #8a8e93;
  transition: 0.3s ease-in-out;
}
.menu-list a:hover {
  color: #60d8f8;
}
.active a {
  color: #ffffff;
  text-decoration: underline;
}
.input-todo {
  margin-top: 20px;
}
.btn-add {
  background-color: #60d8f8;
  color: #ffffff;
  transition: 0.3s ease-in-out;
}
.btn-add:hover {
  background-color: #1dc6f4;
}
.error-message {
  display: inline-block;
  margin: 0;
  color: rgb(255, 47, 47);
}

@media (max-width: 768px) {
  .menu-wrapper {
    padding-left: 0;
  }
  .input-todo {
    margin-top: 8px;
  }
}
</style>

<script>
import { computed, ref, watchEffect } from "vue";
export default {
  props: {
    totalTasks: {
      type: Number,
      default: 0,
    },
  },
  setup(props, { emit }) {
    const task = ref("");
    const errors = ref("");
    const activeTab = ref(1);

    const currentDate = computed(() => {
      let date = new Date();
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const day = days[date.getDay()];
      const month = months[date.getMonth()];
      return `${day}, ${month} ${date.getDate()}`;
    });

    watchEffect(() => {
      if (task.value.trim().length) {
        errors.value = "";
      }
    });

    const addTask = () => {
      if (!task.value.trim().length) {
        errors.value = "This field is required.";
        return;
      }
      emit("addTask", task.value);
      task.value = "";
      errors.value = "";
    };

    const changeTab = (value) => {
      activeTab.value = value;
      emit("changeTab", value);
    };

    return { task, errors, currentDate, addTask, activeTab, changeTab };
  },
};
</script>
