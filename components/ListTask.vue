<template>
  <div>
    <div class="main-block">
      <div
          v-for="category in categories.slice(0, 3)"
          :key="category.name_category"
          @drop="onDrop($event, category.name_category)"
          class="cards"
          @dragover.prevent
          @dragenter.prevent
      >
        <h2>
          {{ category.title_category }}
          (
          {{ tasks.filter(x => x.category_task === category.name_category).length }}
          )
        </h2>
        <div style="display: inline-block"
             v-for="task in tasks.filter(x => x.category_task === category.name_category)"
             @dragstart="onDragStart($event, task)"
             :key="task.number_task"
             v-bind:task="task"
             draggable="true"
             class="card-item"
        >
          <CardTask
              v-bind:task="task"
              @change-status="changeStatus"
              @edit-task="editTask"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import CardTask from "@/components/CardTask";

export default {
  name: "ListTask",
  props: ['tasks', 'categories'],
  methods: {
    /**
     * Вызов функции updateTask в компоненте App с параметром number_task.
     *
     * @param {string} number_task Номер задачи.
     */
    changeStatus(number_task) {
      this.$emit('change-status', number_task)
    },
    /**
     * Вызов функции editTask в компоненте App с параметром task.
     *
     * @param {object} task Информация о задаче.
     */
    editTask(task) {
      this.$emit('edit-task', task)
    },
    /**
     * Запись параметров передвигаемой карточки(задачи) в объект событий при старте передвижения.
     *
     * @param {object} event Объект событий.
     * @param {object} task Информация о задаче.
     */
    onDragStart(event, task) {
      event.dataTransfer.setData('categoryTaskStart', task.category_task.toString())
      event.dataTransfer.dropEffect = 'move'
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setData('itemId', task.number_task.toString())
    },
    /**
     * Вызов dragAndDrop() в компоненте App с параметрами о перемещении карточки.
     *
     * @param {object} event Объект событий.
     * @param {object} name_category Категория задачи.
     */
    onDrop(event, name_category) {
      const itemId = parseInt(event.dataTransfer.getData('itemId'))
      const categoryTaskStart = event.dataTransfer.getData('categoryTaskStart')
      this.$emit('dragAndDrop',
          this.tasks.findIndex(x => x.number_task === itemId),
          name_category,
          categoryTaskStart)
    }
  },
  components: {
    CardTask
  },

}
</script>

<style>


.main-block {
  width: 90%;
  display: grid;
  grid-template-columns: repeat(3, minmax(240px, 300px));
  grid-column-gap: 5px;
  justify-content: space-around;
}

.cards {
  min-height: 600px;
  border: var(--color-base-blue) solid 3px;
  padding: 10%;
}

.card-item {
  display: block;
  width: 100%;
}

h2 {
  margin: 0;
  font-size: 35px;
  color: var(--color-title-gray);
}

@media (max-width: 740px) {
  .main-block {
    overflow-x: auto;
    justify-content: space-between;
  }
}
</style>
