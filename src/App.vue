<script>
export default {
  data() {
    return {
      tasks: [],
      text: "",
      showNotDone: false,
      notDoneTasks: []
    }
  },
  computed: {
    buttonShowDOneTasks: function () {
      if (this.showNotDone) {
        return "Показать все задачи"
      } else {
        return "Показать только невыполненные задачи"
      }
    }
  },
  methods: {
    addNewTask() {
      if (this.text == "") {
        return
      }
      let newTask = {
        id: this.tasks.length,
        taskText: this.text,
        isDone: false
      }
      this.tasks.push(newTask)
      this.text = ""
    },
    done(id) {
      this.tasks = this.tasks.map(task => {
        if (task.id == id) {
          task.isDone = !task.isDone
        }
        return task
      })
    },
    deleteTask(id) {
      for (let i = 0; i < this.tasks.length; i++) {
        if (this.tasks[i].id == id) {
          this.tasks.splice(i, 1)
        }
      }
    },
    showOnlyNotDoneTasks() {
      if (!this.showNotDone) {
        this.notDoneTasks = []
        this.tasks.forEach(task => {
          if (task.isDone == false) {
            this.notDoneTasks.push(task)
          }
        })
        this.showNotDone = true
      } else {
        this.showNotDone = false
      }
    }
  }
}
</script>

<template>
  <div class="container">
    <p>ToDo list</p>
    <input type="text" v-model="text" class="inputText" placeholder="Введите текст задачи">
    <button class="button" @click="addNewTask">Добавить задачу</button>
    <button class="button" @click="showOnlyNotDoneTasks">{{ buttonShowDOneTasks }}</button>
    <div v-for="task in tasks" key="task.id" v-if="!showNotDone">
      <p :class="{ done: task.isDone }" @click="done(task.id)">{{ task.taskText }}</p>
      <button class="buttonDelete" @click="deleteTask(task.id)">Удалить</button>
    </div>
    <div v-for="task in notDoneTasks" key="task.id" v-if="showNotDone">
      <p :class="{ done: task.isDone }" @click="done(task.id)">{{ task.taskText }}</p>
      <button class="buttonDelete" @click="deleteTask(task.id)">Удалить</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 70%;
  margin: auto;
  height: min-content;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 15px;
  background-color: aqua;
}

.inputText {
  width: 50%;
  height: 30px;
  border-radius: 6px;
  border: 1px solid black;
  margin-bottom: 10px;
}

.button {
  width: 30%;
  border-radius: 6px;
  border: 1px solid black;
  background-color: cadetblue;
  height: 30px;
  margin-bottom: 10px;
}

.done {
  text-decoration: line-through;
}
</style>
