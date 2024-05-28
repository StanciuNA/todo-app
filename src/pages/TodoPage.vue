<template>
  <q-page class="bg-white" separator bordered>
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="ajouterTache"
        placeholder="Ajouter une tâche"
        dense
        bg-color="white"
        class="col"
        square
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        clickable
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            push
            color="primary"
            round
            flat
            icon="delete"
            @click.stop="deleteTask(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="tasks.length == 0">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">Aucune tâche</div>
    </div>
  </q-page>
</template>

<script>
import { TouchSwipe } from "quasar";

export default {
  name: "TodoPage",
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "Partir dans la cuisine",
          done: false,
        },
        {
          title: "Voir le frigo",
          done: true,
        },
        {
          title: "Conquerir les pâtes",
          done: false,
        },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Etes vous sûr(e) de vouloir supprimer la tâche",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("La tâche a été supprimé");
        });
    },
    ajouterTache() {
      this.tasks.push({ title: this.newTask, done: false });
      this.newTask = "";
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
.no-tasks {
  opacity: 0.5;
}
</style>
