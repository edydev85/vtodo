<template lang="pug">
  #app
    h4(class="bg-primary text-white text-center p-2" ) {{name}}'s To do List
    .container-fluid.p-4
      .row.py-2
        .col
          input.form-control(type='text' v-model="newItemText" )
        .col-2
          button.btn.btn-primary(@click="addNewTodo") Add
      br    
      .row(v-if="filteredTasks.length == 0" )
        .col.text-center
          b Nothing to do! uhuu
      <template v-else>
        .row
          div(class="col font-weight-bold" ) Task 
          div(class="col-2 font-weight-bold" ) Done
        .row(v-for="t in filteredTasks" :key="t.action")
          .col {{ t.action }}
          .col-2
            input(type='checkbox' class="form-check-input" v-model="t.done" ) 
            | {{t.done}}
      </template>
      .row.bg-secondary.py-2.mt-2.text-white
        .col.text-center 
          .form-check
            input.form-check-input#my-input(type='checkbox' v-model="hideCompleted" )
            label.form-check-label(for='my-input') Hide completed tasks
        .col.text-center
            button.btn.btn.sm.btn-warning(@click="deleteCompleted") Delete

</template>

<script>
export default {
  name: "app",
  data() {
    return {
      name: "Edy",
      tasks: [],
      hideCompleted: true,
      newItemText: ""
    };
  },
  computed: {
    filteredTasks() {
      return this.hideCompleted ? this.tasks.filter(t => !t.done) : this.tasks;
    }
  },
  methods: {
    addNewTodo() {
      this.tasks.push({
        action: this.newItemText,
        done: false
      });
      this.storeData();
      this.newItemText = "";
    },
    storeData() {
      localStorage.setItem("todos", JSON.stringify(this.tasks));
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter(t => !t.done);
      this.storeData();
    }
  },
  created() {
    let data = localStorage.getItem("todos");
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  }
};
</script>
