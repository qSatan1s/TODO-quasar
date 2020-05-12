<template>
  <q-page class="bg-gray-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        class="col"
        square=""
        filled
        bg-color="white"
        placeholder="Add task"
        @keyup.enter="addTask"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white " separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        :class="{ 'done bg-blue-1': task.done }"
        @click="task.done = !task.done"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no=pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            color="primary"
            dense
            @click.stop="deleteTask(index)"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <div class="text-h5 text-primary text-center">
        <q-icon name="check" size="100px" color="primary"></q-icon>
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "Get bananas",
          done: false
        },
        {
          title: "Eat bananas",
          done: false
        },
        {
          title: "Poo bananas",
          done: false
        }
      ]
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really delete?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Task deleted",
            color: "red"
          });
        });
    },

    addTask() {
      this.tasks.push({ title: this.newTask, done: false });
      this.newTask = "";
      this.$q.notify({
        message: "Task add",
        color: "green"
      });
    }
  }
};
</script>

<style scoped>
.done q-item-label {
  text-decoration: line-through;
  color: #bbb;
}

.no-task {
  opacity: 0.5;
}
</style>
