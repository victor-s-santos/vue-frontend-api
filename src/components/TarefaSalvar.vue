<template>
    <div class="mt-4">
        <hr>
        <h2 class="font-weight-light">Salvar Tarefa</h2>
        <form @submit.prevent="salvar">
            <div class="row">
                <div :class="classeColuna">
                    <div class="form-group">
                        <label>Titulo da Tarefa</label>
                        <input 
                            type="text"
                            class="form-control"
                            placeholder="titulo da tarefa"
                            v-model="tarefaLocal.titulo">
                    </div>
                </div>
            <div class="col-sm-2" v-if="tarefa">
                <div class="form-group">
                    <label>Tarefa Concluida?</label>
                    <button
                        :class="classeBtn"
                        @click="tarefaLocal.feito = !tarefaLocal.feito">
                        <i class="fa fa-check"></i>
                    </button>
                </div>
            </div>
        </div>
        <button type="submit" class="btn btn-primary">Salvar Tarefa</button>
        </form>
    </div>               
</template>
<script>
export default {
    props:{
        tarefa:{
            type: Object,
            default: undefined
        }
    },
    data(){
        return{
            //assim não se causa mutações, o que seria chamar tarefa novamente
            tarefaLocal: Object.assign(
                {},
                {titulo:'', feito: false},
                this.tarefa)
        }
    },
    computed:{
        classeBtn(){
            return this.tarefa && this.tarefaLocal.feito
            ? 'btn btn-success btn-sm d-block'
            : 'btn btn-secondary btn-sm d-block'
        },
        classeColuna(){
            return this.tarefa
                ? 'col-sm-10'
                : 'col-sm-12'
        }
    },
    //garante que mude o titulo da tarefa ao se permitar entre elas no momento da edição
    watch: {
        tarefa(tarefaNova, tarefaAntiga){
            this.tarefaLocal = Object.assign({}, this.tarefa)
        }
    },
    methods:{
        salvar(event){
            const operacao = !this.tarefa ? 'criar' : 'editar'
            this.$emit(operacao, this.tarefaLocal)
            this.tarefaLocal = {titulo: '', feito: false}
        }
    }
}
</script>