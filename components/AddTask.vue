<template>
  <div>
    <span>Добавить новую задачу</span>
    <form action="" @submit.prevent="onSubmit">
      <label>
        <input type="text"  placeholder="Описание..." v-model="description">
      </label>
      <button class="add-task-button" type="submit">+</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "AddTasks",
  props: ['tasks'],
  data() {
    return {
      description: ''
    }
  },
  methods: {
    /**
     * Обработка формы с добавлением задачи.
     */
    onSubmit() {
      if (this.description.trim()) {
        const newTask = {
          number_task: this.tasks.length + 1,
          description_task: this.description,
          category_task: "plan"
        }
        this.$emit('add-task', newTask)
        this.description = ''
      }

    }
  }
}
</script>

<style>

span {
  color: var(--color-base-blue);
  font-weight: bold;
}

form {
  margin-top: 10px;
}

input {
  width: 300px;
  color: var(--color-base-blue);
  background-color: var(--color-background-blue);
  border: var(--color-base-blue) solid 1px;
  height: 40px;
  margin-right: 20px;
  font-size: 25px;
}

input::placeholder {
  width: 200px;
  color: var(--color-base-blue);
  padding-left: 15px;
  font-size: 20px;
}

.add-task-button {
  background-color: var(--color-base-blue);
  border: none;
  border-radius: 50%;
  color: white;
  font-size: 24px;
  height: 35px;
  width: 35px;
  cursor: pointer;
}

.add-task-button:active, .add-task-button:focus {
  outline: none;
}

@media (max-width: 440px) {
  input {
    width: 72%;
  }
}

</style>
