<template>
  <div class="modal">
    <div class="modal-close">
      <button
          v-on:click="$emit('close-modal')"
      >&#10006;
      </button>
    </div>
    <div class="modal-content">
      <form action="" @submit.prevent="onSaveTask">
        <label for="edit-description"> Описание</label><br>
        <input type="text" id="edit-description"
               v-model="valueTask.description_task"
               placeholder="Описание..."
        ><br><br>
        <label for="edit-status">Статус</label><br>
        <select name="" id="edit-status" v-model="nameCategory">
          <option value="plan" :selected="nameCategory === 'plan' ? 'selected' : null">План</option>
          <option value="work" :selected="nameCategory === 'work' ? 'selected' : null">В работе</option>
          <option value="ready" :selected="nameCategory === 'ready' ? 'selected' : null">Готово</option>
        </select><br><br>
        <label for="edit-employee">Ответственный</label><br>
        <input type="text" id="edit-employee" placeholder="Введите имя..." v-model="valueTask.name_employee"><br><br>
        <label for="edit-time-start">Дата и время начала</label><br>
        <input
            type="datetime-local"
            id="edit-time-start"
            v-model="valueTask.time_start"
        ><br><br>
        <label for="edit-time-total">Дата и время завершения</label><br>
        <input
            type="datetime-local"
            id="edit-time-total"
            v-model="valueTask.time_finish"
        ><br>

        <button type="submit">
          Сохранить
        </button>
      </form>
    </div>
  </div>
</template>

<!--:value="taskEdit.time_start"-->

<script>
export default {
  name: "ChangingTask",
  props: ['taskEdit', 'categories'],
  data() {
    return {
      nameCategory:
        this.categories.find(category => category.name_category === this.taskEdit.category_task).name_category,
      valueTask: {
        name_category: this.taskEdit.name_category,
        description_task: this.taskEdit.description_task,
        time_start: this.taskEdit.time_start,
        time_finish: this.taskEdit.time_finish,
        name_employee: this.taskEdit.name_employee,
      }
    }
  },
  methods: {
    /**
     * Вызов методов close-modal,
     *               editTask в компоненте ChangingTask с параметром информации о задаче.
     */
    onSaveTask() {
      if (this.nameCategory === 'plan' && this.valueTask.description_task.trim()) {
        this.$emit('edit-task',
            this.taskEdit.number_task,
            this.nameCategory,
            this.valueTask.description_task)
        this.$emit('close-modal')
      }
      if (this.nameCategory === 'work'
          && this.valueTask.description_task.trim()
          && this.valueTask.time_start.trim()) {
        this.$emit('edit-task',
            this.taskEdit.number_task,
            this.nameCategory,
            this.valueTask.description_task,
            new Date(this.valueTask.time_start.toString()))
        this.$emit('close-modal')
      }
      if (this.nameCategory === 'ready'
          && this.valueTask.description_task.trim()
          && this.valueTask.time_start.trim()
          && this.valueTask.time_finish.trim()
          && this.valueTask.name_employee.trim()
      ) {
        this.$emit('edit-task',
            this.taskEdit.number_task,
            this.nameCategory,
            this.valueTask.description_task,
            new Date(this.valueTask.time_start.toString()),
            new Date(this.valueTask.time_finish.toString()),
            this.valueTask.name_employee,
        )
        this.$emit('close-modal')
      }
    }
  }
}
</script>

<style scoped>

.modal {
  z-index: 9999;
  background: var(--color-background);
  border-radius: 20px;
  border: var(--color-title-gray) solid 2px;
  width: 35%;
  position: absolute;
  padding: -20px -20px 0 0;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.modal-close {
  text-align: right;
  margin: 20px;

}

.modal-close button {
  background-color: var(--color-base-blue);
  border: var(--color-base-blue) solid 1px;
  border-radius: 50%;
  height: 35px;
  width: 36px;
  font-size: 22px;
  color: white;
  cursor: pointer;
}

.modal-close button:active, .modal-close button:focus {
  outline: none;
}

.modal-content {
  margin: 50px 100px 50px 100px;
  display: flex;
  justify-content: center;
}

.modal-content label {
  display: inline-block;
  color: var(--color-base-blue);
  font-weight: bold;
  margin-bottom: 5px;
}

.modal-content input {
  padding-left: 5px;
  margin: 0 auto;
  font-size: 18px;
  color: var(--color-base-blue);
}

.modal-content select {
  padding-left: 5px;
  width: 300px;
  background-color: var(--color-background-blue);
  border: var(--color-base-blue) solid 1px;
  border-radius: 5px;
  height: 40px;
  margin-right: 20px;
  font-size: 18px;
  color: var(--color-base-blue);
}

.modal-content input::placeholder{
  padding-left: 5px;
  margin: 0 auto;
  font-size: 18px;
  color: var(--color-base-blue);
}



.modal-content button {
  display: grid;
  margin: 10px auto;
  color: white;
  background-color: var(--color-base-blue);
  border: var(--color-base-blue) solid 2px;
  border-radius: 5px;
  width: 150px;
  font-size: 25px;

}

@media (max-width: 1000px) {
  .modal {
    width: 50%;
  }
}

@media (max-width: 700px) {
  .modal {
    width: 60%;
  }
}

@media (max-width: 600px) {
  .modal {
    width: 90%;
  }

  .modal-content input, .modal-content select {
    width: 250px;
  }

}

</style>
