<template>
  <div class="container-fluid d-flex justify-content-around mt-5 col-sm-12">
    <div class="row">
      <div class="col-sm-3 px-5" v-for="(category,index) in categories" :key="index">
        <div :class="['card', category.name]" style="width: 24rem;">
          <div class="card-header">{{category.name}}</div>
          <div class="card-body pl-5">
            <Task :tasks="category.tasks" :category="category.name.toLowerCase()"/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.Back-Log {
  background-color: red;
  font-size: 18px;
}
.To-Do {
  background-color: orange;
  font-size: 18px;
}
.Ongoing {
  background-color: lightblue;
  font-size: 18px;
}
.Done {
  background-color: lightgreen;
  font-size: 18px;
}
</style>


<script>
import Task from '@/components/Task.vue';
import db from '@/api/api';

export default {
  name: 'category',
  components: { Task },
  data() {
    return {
      categories: [
        { name: 'Back-Log', tasks: null },
        { name: 'To-Do', tasks: null },
        { name: 'On-going', tasks: null },
        { name: 'Done', tasks: null },
      ],
      backlog: [],
      todo: [],
      ongoing: [],
      done: [],
    };
  },
  beforeMount() {
    db.collection('tasks')
      .orderBy('createdAt')
      .onSnapshot(
        (docs) => {
          this.backlog = [];
          this.todo = [];
          this.ongoing = [];
          this.done = [];

          docs.forEach((task) => {
            switch (task.data().status) {
              case 'Back-Log':
                this.backlog.push({ data: task.data(), id: task.id });
                break;
              case 'To-Do':
                this.todo.push({ data: task.data(), id: task.id });
                break;
              case 'On-Going':
                this.ongoing.push({ data: task.data(), id: task.id });
                break;
              case 'Done':
                this.done.push({ data: task.data(), id: task.id });
                break;
              default:
                break;
            }
          });
          this.categories[0].tasks = this.backlog;
          this.categories[1].tasks = this.todo;
          this.categories[2].tasks = this.ongoing;
          this.categories[3].tasks = this.done;
        },
        (error) => {
          console.log(error);
        },
      );
  },
};
</script>
