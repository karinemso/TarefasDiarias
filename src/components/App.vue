<template>
  <div id="app">
    <div class="container">
      <div class="inputs">
        <h1>Tarefas do Dia</h1>
        <form action="" @submit.prevent="addTask">

          <input v-model="taskText" type="text" placeholder="O que você precisa fazer?">
          <button type="submit">ENVIAR</button>

        </form>
      </div>

      <div class="tasks">
        <p v-if="tasks.length == 0"> Adicione sua primeira tarefa</p>
        
        <ul>

          <li v-for="(task, index) in tasks"><span :class="{ 'done': task.completed }" @click="done(index)">{{ task.text
              }}</span>
            <div id="actions"><span @click="editTrigger(index)">edit</span><span @click="deleteTask(index)">X</span></div>
          </li>
          
        </ul>
        <div v-if="toedit.edit">
            <form action="" @submit.prevent="edit">
              <label for="">Edite sua tarefa:</label>
              <input v-model="editText" type="text" >
              <button type="submit">ENVIAR</button>

            </form>
          </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      tasks: localStorage.getItem("tasks") ? JSON.parse(localStorage.getItem("tasks")) : [],
      taskText: '',
      isActive: false,
      toedit:{edit:false, task:0},
      editText: ''
    }
  },
  methods: {
    addTask() {
      this.tasks.push({ text: this.taskText, completed: false })
      this.taskText = ""
      localStorage.setItem('tasks', JSON.stringify(this.tasks))

    },
    done(index) {

      this.tasks[index].completed = !this.tasks[index].completed;
    },
    deleteTask(index) {
      this.tasks = this.tasks.splice(index, 1).length > 0 ? this.tasks.splice(index, 1) : []
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
    ,
    editTrigger(index) {
      if(this.toedit.edit){
        this.toedit = {
        edit:false,
        task: 0
        
      }
      return
      }
      this.toedit = {
        edit:true,
        task: index
      }
      this.editText = this.tasks[index].text
    },
    edit() {
      this.tasks[this.toedit.task].text = this.editText
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
      this.toedit = {
        edit:false,
        task: 0
      }
    }
  }
}
</script>

<!-- CSS libraries -->

<style src="normalize.css/normalize.css"></style>

<!-- Global CSS -->

<style>
code {
  font-family: Menlo, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono, Bitstream Vera Sans Mono, Courier New, monospace, serif;
  font-size: 0.9em;
  white-space: pre-wrap;
  color: #2c3e50;
}

code::before,
code::after {
  content: '`';
}
</style>

<!-- Scoped component css -->
<!-- It only affect current component -->

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}


#actions {
  display: flex;
  gap: 1em;
}
#app {
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  font-family: Arial, Helvetica, sans-serif;
}


body {
  background-color: #2D033B;
}

.tasks,
.inputs {
  background-color: #810CA8;
  margin-top: 2em;
  border-radius: 10px;
  padding: 2em 0em;
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
  gap: 1em;
}



.tasks ul li {
  padding: 1em;
  background-color: #C147E9;
  color: white;
  border-radius: 10px;
  list-style: none;
  min-width: 60vw;
  text-align: left;
  display: flex;
  justify-content: space-between;
}

.tasks ul li span {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
  opacity: 0.5;
}

.tasks ul {
  display: flex;
  gap: 1em;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 0
}

.inputs h1 {
  color: black;
}

 input {
  width: 90%;
  padding: 1em;
  border-radius: 10px;
  background-color: black;
  color: dimgrey;
  border: none;
}

 form {
  display: flex;
  gap: 1em;
}

button {
  background-color: black;
  color: #C147E9;
  border-radius: 10px;
  border: none;
  padding: 1em;


}

.inputs button:hover {
  background-color: #C147E9;
  color: black;


}

.container {
  width: 1500px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}




.banner {
  height: 200px;
  background-color: #f6f6f6;
  padding: 50px 10px;
}

.bottom {
  padding: 80px 10px;
  font-size: 24px;
  font-weight: 300;
}

.fade {
  font-size: 14px;
}

.logo {
  animation: spin 4s 1s infinite linear
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}
</style>
