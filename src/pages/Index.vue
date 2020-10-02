<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTodo()"
        v-model="newTodo"
        class="col"
        square
        bg-color="white"
        filled
        placeholder="Adicionar TODO"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTodo()" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white">
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
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTasks(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!this.tasks.length" class="no-tasks absolute-center">
      <q-icon 
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
        Nenhum TODO
      </div>
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
      };

      this.tasks.push({ title: this.newTodo, done: false });
      this.newTodo = "";
    },
    deleteTasks(index) {
      this.$q
        .dialog({
          title: "Confirme",
          message: "Deseja deleter este TODO?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "TODO deletado.",
            color: "black",
          });
        });
    },
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks{
  opacity: 0.5;
}
</style>