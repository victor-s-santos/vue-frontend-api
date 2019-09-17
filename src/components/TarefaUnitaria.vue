<template>
    <li class="list-group-item d-flex">
        <span>{{ tarefa.titulo }}</span>
        <span class="espacar"></span>
        <button class="btn btn-sm mr-4" :class="classeCSS" :title="tituloBtnConcluido">
            <i class="fa fa-check"></i>
        </button>
        <button 
            class="btn btn-primary btn-sm mr-1" 
            title="Editar"
            @click="$emit('editar', tarefa)">
                <i class="fa fa-pencil-alt"></i>
        </button>
        <button class="btn btn-danger btn-sm" title="Excluir">
            <i class="fa fa-trash"></i>
        </button>
    </li>
</template>

<script>
export default {
    props:{
        tarefa:{
            type: Object,
            required: true
        }
    },
    computed:{
        classeCSS(){
            return{
                'btn-secondary': !this.tarefa.feito,
                'btn-sucess': this.tarefa.feito
            }
        },
        tituloBtnConcluido(){
            return this.tarefa.feito
                ? 'Refazer Tarefa'
                : 'Tarefa Realizada'
        }
    }
    
}
</script>
<style scoped>
    .espacar{
        flex: 1 1 auto;
    }
</style>