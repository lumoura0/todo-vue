<script setup>
const props = defineProps([
  "cadastraTarefa",
  "tarefaTemp",
  "trocaFiltro",
  "editaTarefaTemp",
]);
</script>

<template>
  <form @submit.prevent="props.cadastraTarefa">
    <div class="row">
      <div class="col">
        <input
          :value="props.tarefaTemp"
          @change="props.editaTarefaTemp"
          required
          class="form-control"
          type="text"
          name=""
          id=""
          placeholder="Digite aqui a descrição da tarefa"
        />
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary" type="submit">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select class="form-control" @change="props.trocaFiltro">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
</template>

<style>
@media screen and (max-width: 768px) {
  button,
  select {
    margin-top: 15px;
  }
}
</style>
