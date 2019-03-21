<template>
  <div>
    <div class="container col-sm-12" id="navbar">
      <div class="row">
        <div class="col-sm-6">
          <h4>KANBAN BOARD</h4>
        </div>
        <div class="col-sm-6 d-flex justify-content-end">
          <button data-toggle="modal" data-target="#modalNewTask" class="btn btn-warning">
            New Task
          </button>
        </div>
      </div>
    </div>
    <div class="modal fade" id="modalNewTask" tabindex="-1" role="dialog" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">New Task</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="title" class="col-form-label">Title</label>
                <input v-model="title" type="text" class="form-control" placeholder="Task Title">
              </div>
              <div class="form-group">
                <label for="description" class="col-form-label">Description</label>
                <textarea
                  v-model="description"
                  class="form-control"
                  placeholder="Task Short Description"
                ></textarea>
              </div>
              <div class="form-group">
                <label for="point" class="col-form-label">Points</label>
                <input v-model="point" type="text" class="form-control">
              </div>
              <div class="form-group">
                <label for="assigned-to" class="col-form-label">Assigned To</label>
                <input
                  v-model="assigned"
                  type="text"
                  class="form-control"
                  placeholder="Assigned To"
                >
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Cancel</button>
            <button @click="submitNewTask" type="button" class="btn btn-primary">Save</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
#navbar {
  padding: 25px;
  background-color: #000;
  color: #fff;
}

.modal-content {
  background-color: #343a40;
  color: #fff;
}

.modal-footer {
  color: #fff;
}

h4 {
  padding-top: 5px;
}
</style>
<script>
import db from '@/api/api';

// const $ = null;

export default {
  name: 'navbar',
  data() {
    return {
      title: '',
      description: '',
      point: 0,
      assigned: '',
    };
  },
  methods: {
    submitNewTask() {
      db.collection('tasks')
        .add({
          title: this.title,
          description: this.description,
          point: this.point,
          assigned: this.assigned,
          status: 'Back-Log',
          createdAt: new Date(),
        })
        .then((docRef) => {
          console.log('Document written with ID: ', docRef.id);
          this.title = null;
          this.description = null;
          this.point = 0;
          this.assigned = null;
        })
        .catch((error) => {
          console.error('Error adding document: ', error);
        });
      $('#modalNewTask').modal('toggle');
    },
  },
};
</script>
