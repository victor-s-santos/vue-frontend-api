<template>
    <li class="list-group-item d-flex">
        <span>{{ tarefa.titulo }}</span>
        <span class="espacar"></span>
        <button 
            :class="classeCSS" 
            :title="tituloBtnConcluido"
            @click="concluirTarefa">
            <i class="fa fa-check"></i>
        </button>
        <button 
            class="btn btn-primary btn-sm mr-1" 
            title="Editar"
            @click="$emit('editar', tarefa)">
                <i class="fa fa-pencil-alt"></i>
        </button>
        <button 
            class="btn btn-danger btn-sm" 
            title="Excluir"
            @click="$emit('deletar', tarefa)">
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
                'btn btn-secondary btn-sm d-block': !this.tarefa.feito,
                'btn btn-success btn-sm d-block': this.tarefa.feito
            }
        },
        tituloBtnConcluido(){
            return this.tarefa.feito
                ? 'Refazer Tarefa'
                : 'Tarefa Realizada'
        }
    },
    methods:{
        concluirTarefa(){
            this.$emit('concluir', Object.assign({}, this.tarefa, { feito: !this.tarefa.feito }))
        }
    }
    
}
</script>
<style scoped>
    .espacar{
        flex: 1 1 auto;
    }
</style>