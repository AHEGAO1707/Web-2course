<template>
  <div>
    <div id="app">
      <div class="header">
        <div class="title-kanban">
          <h1>Канбан</h1>
        </div>
        <div class="edit-schema">
          <p>Темная тема</p>
          <toggle-button
              @change="editSchema"
              color="black"
              :labels="true"
              :font-size="18"
              :width='60'
              :height="30"
          />
        </div>
      </div>
      <AddTask
          @add-task="addTask"
          v-bind:tasks="tasks"
      />
      <ListTask
          @change-status="updateTask"
          @edit-task="editTask"
          @dragAndDrop="dragAndDrop"
          v-bind:tasks="tasks"
          v-bind:categories="categories"
          class="ListTask"
      />
    </div>
    <div v-if="showModal" class="modal-shadow" @click.self="closeModal">
      <ChangingTask
          v-bind:taskEdit="taskEdit"
          v-bind:categories="categories"
          @close-modal="closeModal"
          @edit-task="updateTask"
      />
    </div>
  </div>
</template>


<script>
import AddTask from "@/components/AddTask";
import ListTask from "@/components/ListTask";
import {ToggleButton} from 'vue-js-toggle-button';
import Vue from 'vue';

Vue.component('ToggleButton', ToggleButton)

export default {
  name: 'App',
  data() {
    return {
      showModal: false,
      clicked: false,
      switch1: true,
      categories: [{
        "id": 0,
        "name_category": "plan",
        "title_category": "План"
      }, {
        "id": 1,
        "name_category": "work",
        "title_category": "В работе"
      }, {
        "id": 2,
        "name_category": "ready",
        "title_category": "Готово"
      }, {
        "id": 3,
        "name_category": "delete",
        "title_category": ""
      }],
      tasks: [],
      taskEdit: {}
    }
  },
  methods: {
    /**
     * Закрытие окна для изменения задачи.
     */
    closeModal() {
      this.showModal = false
      document.getElementById('app').style.pointerEvents = "auto"
    },
    /**
     * Добавляет task в массив задач tasks.
     *
     * @param {object} task Информация о задаче.
     */
    addTask(task) {
      this.tasks.push(task)
    },
    /**
     * Открытие окна для изменения задачи и передача объекта task в форму для изменений.
     *
     * @param {object} task Информация о задаче.
     */
    editTask(task) {
      document.getElementById('app').style.pointerEvents = "none"
      this.showModal = true
      this.taskEdit = task
    },
    /**
     * Обновление объекта задачи параметрами с информацией о задаче по number_task.
     *
     * @param {string} number_task Номер задачи.
     * @param {string} category_task Категория задачи.
     * @param {string} description_task Описание задачи.
     * @param {string} time_start Время начала выполнения задачи.
     * @param {string} time_finish Время завершения выполнения задачи.
     * @param {string} name_employee Имя исполнителя задачи.
     */
    updateTask(number_task, category_task, description_task, time_start, time_finish, name_employee) {
      const newData = new Date();

      let task = this.tasks.find(item => item.number_task === number_task)
      let categoryId = this.categories.find(category => category.name_category === task.category_task).id
      let index = this.tasks.indexOf(task)

      this.tasks[index].category_task =
          category_task ? category_task : this.categories[categoryId + 1].name_category
      this.tasks[index].description_task =
          description_task ? description_task : this.tasks[index].description_task
      this.tasks[index].time_start =
          time_start ? time_start : (task.category_task === 'work') ? newData : this.tasks[index].time_start
      this.tasks[index].time_finish =
          time_finish ? time_finish : (task.category_task === 'ready') ? newData : null
      this.tasks[index].time_total =
          (task.category_task === 'ready') ? newData - this.tasks[index].time_start : null
      this.tasks[index].name_employee =
          name_employee ? name_employee : (task.category_task === 'ready') ? "Имя оответственного" : null
    },
    /**
     * Изменение объекта задачи при переносе с помощью DragAndDrop.
     *
     * @param {string} index Индекс объекта задачи в массиве tasks.
     * @param {string} name_category Категория задачи.
     * @param {string} category_task_start Категория из который был осуществлен перенос задачи.
     */
    dragAndDrop(index, name_category, category_task_start) {
      const newData = new Date();
      this.tasks[index].category_task = name_category
      this.tasks[index].time_start =
          (this.tasks[index].category_task === 'work') ? newData : this.tasks[index].time_start
      this.tasks[index].time_finish =
          (this.tasks[index].category_task === 'ready') ? newData : null
      this.tasks[index].time_total =
          (this.tasks[index].category_task === 'ready') ? newData - this.tasks[index].time_start : null
      this.tasks[index].name_employee =
          (this.tasks[index].category_task === 'ready') ? "Имя оответственного" : null

      this.tasks[index].time_start =
          (this.tasks[index].category_task === 'ready' && category_task_start === 'plan') ? newData :
              this.tasks[index].time_start
      this.tasks[index].time_total =
          (this.tasks[index].category_task === 'ready' && category_task_start === 'plan') ? 1000 :
              this.tasks[index].time_total
    },
    /**
     * Изменение темы.
     */
    editSchema() {
      if (this.clicked) {
        document.documentElement.style.setProperty('--color-title-gray', '#7f7f7f');
        document.documentElement.style.setProperty('--color-background', 'white');
      } else {
        document.documentElement.style.setProperty('--color-title-gray', '#ffffff');
        document.documentElement.style.setProperty('--color-background', '#474747');
      }
      this.clicked = !this.clicked
    }
  },
  components: {
    AddTask,
    ListTask
  }
}
</script>


<style>
:root {
  --color-title-gray: #7f7f7f;
  --color-base-blue: black;
  --color-background-blue: #d4d4d4;
  --color-life-dark: #36556c;
  --color-background: white;
}

body {
  margin: 0;
  background-color: var(--color-background);
}

#app {
  max-width: 90%;
  margin: 0 auto;
}

.header {
  display: flex;
  justify-content: space-between;
}

.edit-schema {
  display: flex;
  align-items: center;
}

.edit-schema p {
  color: var(--color-title-gray);
  margin-right: 5px;
  font-size: 16px;
}

.title-kanban h1 {
  font-size: 40px;
  color: var(--color-title-gray);
}

.ListTask {
  margin-top: 30px;
  display: flex;
  justify-content: center;
}

@media (max-width: 400px) {
  .edit-schema {
    display: flex;
    flex-direction: column;
  }

  .edit-schema p {
    margin-right: 0;
  }
}

</style>


