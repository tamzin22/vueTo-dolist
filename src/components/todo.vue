<template>
    <div class="container">
      <h2>ToDo List</h2> 
      <div class="d-flex">
          <input v-model="task" type="text" class="form-control">
          <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
      </div>
          <!--tasks  -->
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
                  <tr v-for="(task,index) in tasks" :key="index">
                      <td>
                          <span :class="{'done': task.status ==='done'}">{{task.name}}</span>
                      </td>

                      <td>
                          <span class="pointer" @click="changeStatus(index)"
                          :class="{'text-danger': task.status === 'to-do',
                                   'text-warning': task.status === 'in-progress',
                                   'text-success': task.status === 'done'}"
                          >
                          {{firstCharUpper(task.status)}}
                          </span>
                     </td>

                      <td>
                          <div class="text-center" @click="editTask(index)">
                              <span class="fa fa-pen"></span>
                          </div>
                      </td>

                      <td>
                          <div class="text-center" @click="deleteTask(index)">
                              <span class="fa fa-trash"></span>
                          </div>
                      </td>

                  </tr>
              </tbody>
           </table>
    </div>
</template>

<script>
export default {
  data() {
      return {
          task:'',
          editedTask:null,
          availableStatuses:['to-do','in-progress','done'],

          tasks:[
              {
                  name   : 'Create Todo List App',
                  status : 'to-do'
              },
              {
                  name   : 'Clean and wash',
                  status : 'to-do'
              },
              {
                  name   : 'Start project for Eugene',
                  status : 'to-do'
              },
              {
                  name   : 'Sleep till I drop',
                  status : 'to-do'
              }
          ]
      }
  }, 
  methods:{
      submitTask(){
          if(this.task.length === 0)return;

          if (this.editedTask === null) {

            this.tasks.push({
              name : this.task,
              status : 'to-do'
          });  
          }else{
              this.tasks[this.editedTask].name = this.task;
              this.editedTask = null;
          }
          
          this.task = '';
      },

      deleteTask(index){
          this.tasks.splice(index,1);
      },

      editTask(index){
          this.task = this.tasks[index].name;
          this.editedTask = index;
      },

      changeStatus(index){
          //find current index
       let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
        if (++newIndex > 2) newIndex = 0;
        //to change status
        this.tasks[index].status = this.availableStatuses[newIndex];
      },
      firstCharUpper(str){

          return str.charAt(0).toUpperCase() + str.slice(1);
      }
  } 
}
</script>

<style>
   .pointer{
       cursor: pointer;
   }
   .done{
       text-decoration: line-through;
   }
</style>