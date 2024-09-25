<template>
<div class="container">
  <h2 class="text-center mt-5">My Vue To Do App</h2>


<div class="d-flex">
  <input v-model="task" type="text" class="form-control" placeholder="Enter Text">
  <button @click="submitTask()" class="btn btn-warning rounded-0">Submit</button>
</div>

<table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col">#</th>
      <th scope="col">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td><span :class="{'finished': task.status === 'finished'}">{{task.name}}</span></td>
      <td><span @click="changeStatus(index)" 
        class="pointer"
        :class="{'text-danger': task.status === 'to-do',
        'text-warning': task.status === 'in-progress',
        'text-success': task.status === 'finished'  
        }">{{ firstCharupper(task.status) }}</span></td>
      <td>
        <div @click="editTask(index)">
          <span>Edit</span>
        </div>
      </td>
      <td>
        <div @click="deleteTask()">
          <span>Delete</span>
        </div>
      </td>
    </tr>
  </tbody>
</table>
</div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Steal bananas from the store',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour',
          status: 'in-progress'
        }
      ]
    }
  },

  methods: {
    submitTask(){
      if(this.task.length === 0) return;
      if(this.editedTask === null){
      this.tasks.push({
        name: this.task,
        status: 'to-do'
      });
    } else {
      this.tasks[this.editedTask].name = this.task;
      this.editedTask = null;
    }
      this.task = '';
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharupper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>

<style>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
