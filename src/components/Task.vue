<template>
  <div>
    <div
      class="card text-white bg-dark mb-3"
      style="max-width: 18rem;"
      v-for="(task, index) in tasks"
      :key="index"
    >
      <div class="card-header">
        <b>{{task.data.title}}</b>
      </div>
      <div class="card-body">
        <p class="card-text">Points: {{task.data.point}}</p>
        <p class="card-text">Assigned To: {{task.data.assigned}}</p>
      </div>
      <div class="card-footer">
        <button
          class="btn btn-primary"
          @click="sendDetails(task.id)"
          data-toggle="modal"
          :data-target="'#showDetail' + category"
        >Show Details</button>
      </div>
    </div>
    <div
      class="modal fade"
      :id="'showDetail' + category"
      tabindex="-1"
      role="dialog"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Details Task: {{title}} for {{assigned}}</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p>
              <b>Task Description :</b>
            </p>
            <p>{{description}}</p>
            <p>
              <b>Points :</b>
            </p>
            <p>{{point}}</p>
            <p>
              <b>Status :</b>
            </p>
            <p>{{status}}</p>
          </div>
          <div class="modal-footer d-flex justify-content-between">
            <button
              class="btn btn-warning"
              @click="moveTask('Back-Log')"
              v-if="'status' === 'To-Do'"
            >Back-Log</button>
            <button
              class="btn btn-secondary"
              @click="moveTask('To-Do')"
              v-if="status === 'On-Going'"
            >To-Do</button>
            <button
              class="btn btn-warning"
              @click="moveTask('Back-Log')"
              v-if="status === 'To-Do'"
            >Back-Log</button>
            <button
              class="btn btn-primary"
              @click="moveTask('On-Going')"
              v-if="status === 'Done'"
            >On-Going</button>
            <button class="btn btn-danger" @click="deleteTask">Delete</button>
            <button
              class="btn btn-secondary"
              @click="moveTask('To-Do')"
              v-if="status === 'Back-Log'"
            >To-Do</button>
            <button
              class="btn btn-primary"
              @click="moveTask('On-Going')"
              v-if="status === 'To-Do'"
            >On-Going</button>
            <button
              class="btn btn-success"
              @click="moveTask('Done')"
              v-if="status === 'On-Going'"
            >Done</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-content {
  padding: 10px 20px;
  background-color: #343a40;
  color: #fff;
}
</style>

<script>
import db from '@/api/api';

// const $ = null;

export default {
  name: 'task',
  data() {
    return {
      title: '',
      description: '',
      assigned: '',
      point: '',
      id: '',
      status: '',
    };
  },
  methods: {
    sendDetails(id) {
      this.tasks.forEach((task) => {
        if (task.id === id) {
          this.title = task.data.title;
          this.description = task.data.description;
          this.assigned = task.data.assigned;
          this.point = task.data.point;
          this.status = task.data.status;
          this.id = id;
        }
      });
    },
    deleteTask() {
      db.collection('tasks')
        .doc(this.id)
        .delete()
        .then((docs) => {
          console.log(docs);
        })
        .catch((err) => {
          console.log(err);
        });
      $(`#showDetail${this.category}`).modal('toggle');
    },
    moveTask(status) {
      db.collection('tasks')
        .doc(this.id)
        .update({
          status,
        })
        .then((docs) => {
          console.log(docs);
        })
        .catch((err) => {
          console.log(err);
        });
      $(`#showDetail${this.category}`).modal('toggle');
    },
  },
  props: ['tasks', 'category'],
};
</script>
