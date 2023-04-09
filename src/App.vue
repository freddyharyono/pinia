<template>
  <main>

    <!-- heading -->
    <header>
      <img src="https://pinia.vuejs.org/logo.svg" alt="pinia logo">
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All Tasks</button>
      <button @click="filter = 'favs'">Fav Tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="taskStore.loading">Loading Tasks</div>


    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>Your Have {{ totalCount }} tasks todo</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task "/>
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>Your Have {{ favCount }} tasks todo</p>
      <div v-for="task in favs">
        <TaskDetails :task="task "/>
      </div>
    </div>

    <button @click="taskStore.$reset">reset state</button>
  </main>
</template>

<script>
import { ref } from 'vue';
import TaskDetails from './components/TaskDetails.vue' 
import TaskForm from './components/TaskForm.vue' 
import { useTaskStore } from './stores/TaskStore';
import { storeToRefs } from 'pinia';

  export default {
    components: {TaskDetails, TaskForm},
    setup() {
      const taskStore = useTaskStore()
      const {tasks, loading, favs, totalCount, favCount} = storeToRefs(taskStore)
      taskStore.getTasks()
      const filter = ref('all')
      return {taskStore, filter, tasks, loading, favs, totalCount, favCount} 
    }
  }
</script>