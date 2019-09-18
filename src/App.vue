<template>
  <div id="app">
    
    <div class="jumbotron jumbotron-fluid">
      <div class="container">
        <h1 class="display-4">Frontend de um Crud, em Vue Js</h1>
        <p class="lead">Usando a biblioteca Axios, executando os métodos CRUD e uma api falsa gerada pelo Json-server.</p>
      </div>
    </div>

    <div class="container">
      <ListaTarefas />
    </div>
  </div>
</template>
<script>
import ListaTarefas from './components/ListaTarefas.vue'
export default {
  components:{
    ListaTarefas
  }
}
</script>