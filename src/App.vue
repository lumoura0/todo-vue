<script setup>
import { reactive, onMounted, onBeforeUnmount } from "vue";
import Cabecalho from "./components/Cabecalho.vue";
import Formulario from "./components/Formulario.vue";
import ListaDeTarefas from "./components/ListaDeTarefas.vue";

const estado = reactive({
  filtro: "todas",
  tarefaTemp: "",
  tarefas: [],
});

const getTarefasPendentes = () => {
  return estado.tarefas.filter((tarefa) => !tarefa.finalizada);
};

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter((tarefa) => tarefa.finalizada);
};

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case "pendentes":
      return getTarefasPendentes();
    case "finalizadas":
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
};

const cadastrarTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  };
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = "";
  salvarNoLocalStorage();
};

const salvarNoLocalStorage = () => {
  localStorage.setItem("tarefas", JSON.stringify(estado.tarefas));
};

const carregarDoLocalStorage = () => {
  const tarefasSalvas = localStorage.getItem("tarefas");
  if (tarefasSalvas) {
    estado.tarefas = JSON.parse(tarefasSalvas);
  }
};

onMounted(() => {
  carregarDoLocalStorage();
});

onBeforeUnmount(() => {
  salvarNoLocalStorage();
});
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario
      :troca-filtro="(evento) => (estado.filtro = evento.target.value)"
      :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="(evento) => (estado.tarefaTemp = evento.target.value)"
      :cadastra-tarefa="cadastrarTarefa"
    />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>

<style scoped>
header {
  background-image: url("https://images.unsplash.com/photo-1484480974693-6ca0a78fb36b?q=80&w=2072&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center right;
}
</style>
