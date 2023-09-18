<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    tarefaTemporaria: '',
    filtro:'todas',
    tarefas:[{
      titulo:'estudar Vue',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada:false,
    },
    {
      titulo: 'estudar LESS',
      finalizada:true,
    }
  ]
  })

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemporaria,
      finalizada: false
    }
      estado.tarefas.push(tarefaNova);
      estado.tarefaTemporaria = "";
  }

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada == false)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada == true)
  }

  const getTarefasFiltradas = () => {
    const {filtro} = estado;
    switch(filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas(); 
      default:
        return estado.tarefas
    } 
  }
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
  </div>
  <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemporaria" @change="e => estado.tarefaTemporaria = e.target.value" required type="text" placeholder="Digite sua tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="e => estado.filtro = e.target.value" class="form-control">
          <option value="todas">todas tarefas</option>
          <option value="pendentes">pendentes</option>
          <option value="finalizadas">finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input @change="e => tarefa.finalizada = e.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{done: tarefa.finalizada == true}" class="ms-3" :for="tarefa.titulo">
        {{tarefa.titulo}}
      </label>
    </li>
  </ul>
</template>

<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>
