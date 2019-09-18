<template>
    <div>
        <div class="row">
            <div class="col-sm-10">
                <h1 class="font-weight-light">Lista de Tarefas</h1>
            </div>
            <div class="col-sm-2">
                <button 
                class="btn btn-primary-float-right"
                @click="exibirFormCriarTarefa">
                   <i class="fa fa-plus mr-2"></i>
                   <span>Criar!</span>
                </button>
            </div>
        </div>
        
        <ul class="list-group" v-if="tarefasOrdenadas.length > 0">
            <TarefaUnitaria
                v-for="tarefa in tarefasOrdenadas"
                :key="tarefa.id"
                :tarefa="tarefa"
                @editar="selecionarTarefaEdicao"
                @deletar="deletarTarefa"
                @concluir="editarTarefa"/>
        </ul>
        <li v-else-if="!msgError">Nenhuma tarefa cadastrada!</li>
        <div class="alert alert-danger" v-else>{{ msgError}}</div>
        <TarefaSalvar 
           v-if="exibirFormulario"
            :tarefa="tarefaSelecionada"
           @criar="criarTarefa" 
           @editar="editarTarefa"/>
    </div>
</template>
<script>
import axios from 'axios'
import TarefaSalvar from './TarefaSalvar.vue'
import TarefaUnitaria from './TarefaUnitaria.vue'

import config from './../config/config'
export default {
    components:{
        TarefaSalvar,
        TarefaUnitaria
    },
    data(){
        return{
            tarefas:[],
            exibirFormulario: false,
            tarefaSelecionada: undefined,
            msgError: undefined,
        }
    },
    created(){
        axios.get(`${config.apiURL}/tarefas`)
            .then((response) => {
                //console.log('GET /tarefas',response)
                this.tarefas = response.data
            }).catch(error => {
                console.log(`Algo de errado não está certo: ${error}!`)
                if(error.response){
                    this.msgError = `O servidor acusou o seguinte erro: ${error.message} ${error.response.statusText}`
                    console.log(`Erro ${error.response}`)
                }else if(error.request){
                    this.msgError = `Erro ao tentar se comunicar com o servidor: ${error.message}`
                    console.log(`Erro ${error.request}`)
                }else{
                    this.msgError = `Erro ao fazer requisição ao servidor: ${error.message}`
                }
            })
        },
    computed:{
        tarefasOrdenadas(){
            return this.tarefas.slice().sort((tarefa1, tarefa2) => {//ordenando alfabeticamente pelo titulo
                if(tarefa1.feito === tarefa2.feito){
                    return tarefa1.titulo < tarefa2.titulo
                    ? -1
                    : tarefa1.titulo > tarefa2.titulo
                        ? 1
                        : 0
                }
                return tarefa1.feito - tarefa2.feito//isto já ordena por feito ou não feito
            })
        }
    },
    methods:{
        criarTarefa(tarefa){
            axios.post(`${config.apiURL}/tarefas`, tarefa)
                .then((response) => {
                    //console.log('POST /tarefas', response)
                    this.tarefas.push(response.data)
                    //this.exibirFormulario = !this.exibirFormulario
                    this.resetar()
                })
        },
        editarTarefa(tarefa){
            axios.put(`${config.apiURL}/tarefas/${tarefa.id}`, tarefa)
                .then(() => {
                    const indice = this.tarefas.findIndex(t => t.id === tarefa.id)
                    this.tarefas.splice(indice, 1, tarefa)
                    this.resetar()
                })
        },
        deletarTarefa(tarefa){
            const confirmar = window.confirm(`Tem certeza que gostaria de deletar a tarefa ${tarefa.titulo}?`)
            if(confirmar){
                axios.delete(`${config.apiURL}/tarefas/${tarefa.id}`)
                    .then(() => {
                        //console.log(`DELETE /tarefas/${tarefa.id}`, response)
                        const indice = this.tarefas.findIndex(t => t.id === tarefa.id)
                        this.tarefas.splice(indice, 1)
                    })
            }
        },
        resetar(){
            this.tarefaSelecionada = undefined
            this.exibirFormulario = false
        },
        selecionarTarefaEdicao(tarefa){
            this.tarefaSelecionada = tarefa
            //this.exibirFormulario = !this.exibirFormulario
            //o problema de fazer como acima, é que surge a necessidade de se clicar duas vezes ao 
            //permutar entre as tarefas(uma vez pra ser false, e outra pra ser true)
            this.exibirFormulario = true

        },
        exibirFormCriarTarefa(){
            if(this.tarefaSelecionada){
                this.tarefaSelecionada = undefined
                return
            }
            this.exibirFormulario = !this.exibirFormulario
        }
    }
    }
</script>