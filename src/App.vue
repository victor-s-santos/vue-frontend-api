<template>
  <div id="app">
    
    <div class="jumbotron jumbotron-fluid">
      <div class="container">
        <h1 class="display-4">Requisições HTTP no Vue</h1>
        <p class="lead">Usando a biblioteca Axios para fazer chamadas à uma API REST.</p>
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