<template>
  <q-page class="bg-dark-3 column">
    <div class="row q-pa-sm bg-dark">
      <q-input
        @keyup.enter="addTodo()"
        v-model="newTodo"
        class="col"
        square
        bg-color="black"
        filled
        placeholder="Adicionar TODO"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTodo()" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-dark">
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ done: task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            val="teal"
            color="dark"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTasks(index, task.index)"
            flat
            round
            dense
            color="white"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!this.tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="dark" />
      <div class="text-h5 text-dark text-center">Nenhum TODO</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      tasks: [],
    };
  },
  methods: {
    async load() {
      let allKeys = await this.$q.localStorage.getAllKeys();
      console.log(allKeys);

      for (let i = 0; i < allKeys.length; i++) {
        let Tasks = this.$q.localStorage.getItem(allKeys[i]);

        this.tasks.push(Tasks);

        console.log(Tasks);
      }
    },
    addTodo() {
      let verify = this.tasks.filter((todo) => {
        return todo.title === this.newTodo;
      });

      if (verify.length > 0) {
        this.$q.notify({
          message: "Este TODO já existe.",
          color: "black",
        });
        return;
      }

      let newTask = { title: this.newTodo, done: false };
      let index = this.tasks.push(newTask);
      newTask.index = index.toString();
      this.$q.localStorage.set(index, newTask);
      this.newTodo = "";
    },
    deleteTasks(index, taskIndex) {
      this.$q
        .dialog({
          title: "Confirme",
          message: "Deseja deleter este TODO?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.localStorage.remove(taskIndex);
          this.$q.notify({
            message: "TODO deletado.",
            color: "black",
          });
        });
    },
  },
  created() {
    this.load();
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: dark;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>