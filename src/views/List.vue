<template>
  <div class="container mt-2">
    <template v-if="isTasksEmpty">
      <div class="empty-data mt-2">
        <img src="../assets/images/empty-data.svg" class="empty-data-image">
        <b-button variant="outline-primary" class="mt-2" size="lg" to="/form"> Criar tarefa</b-button>
      </div>
    </template>

    <template v-else>
      <div v-for="(task, index) in tasks" :key="index">
        <b-card :title="task.subject" class="mb-2">
          <b-card-text>{{ task.description }}</b-card-text>

          <b-button
            variant="outline-secondary"
            @click="edit(index)"
            class="mr-2"
          >
            Editar
          </b-button>
          <b-button
            variant="outline-danger"
            @click="remove(task, index)"
            class="mr-2"
          >
            Excluir
          </b-button>
        </b-card>
      </div>
    </template>

    <b-modal ref="modalRemove" hide-footer title="Exclusão de tarefa">
      <div class="d-block text-center">
        Deseja realmente excluir essa tarefa? {{ taskSelected.subject }}
      </div>
      <div class="mt-3 d-flex justify-content-end">
        <b-button variant="outline-secondary" @click="hideModal" class="mr-2">
          Cancelar
        </b-button>
        <b-button
          variant="outline-danger"
          @click="confirmRemoveTask"
          class="mr-2"
        >
          Excluir
        </b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      tasks: [],
      taskSelected: [],
    };
  },

  created() {
    this.tasks = localStorage.getItem("tasks")
      ? JSON.parse(localStorage.getItem("tasks"))
      : [];
  },

  methods: {
    edit(index) {
      this.$router.push({ name: "form", params: { index } });
    },

    remove(task, index) {
      this.taskSelected = task;
      this.taskSelected.index = index;
      this.$refs.modalRemove.show();
    },

    hideModal() {
      this.$refs.modalRemove.hide();
    },

    confirmRemoveTask() {
      this.tasks.splice(this.taskSelected.index, 1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
      this.hideModal();
    },
  },
  computed: {
    isTasksEmpty() {
      return this.tasks.length === 0;
    },
  },
};
</script>
<style scoped>

.empty-data{
 display: flex;
 align-items: center;
 justify-content: center;
 flex-direction: column; 
}
.empty-data-image{
  width: 300px;
  height: 300px;
}

</style>