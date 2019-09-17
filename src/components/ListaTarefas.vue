<template>
    <div>
        <h1 class="font-weight-light">Lista de Tarefas</h1>
        <ul class="list-group" v-if="tarefas.length > 0">
            <TarefaUnitaria
                v-for="tarefa in tarefas"
                :key="tarefa.id"
                :tarefa="tarefa"/>
        </ul>
        <li v-else>Nenhuma tarefa cadastrada!</li>
        <TarefaSalvar />
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
            tarefas:[
                // {id:1, titulo: 'Praticar VueJs', feito: true},
                // {id:1, titulo: 'Praticar NodeJs', feito: true},
                // {id:1, titulo: 'Praticar React-Native', feito: false}

            ]
        }
    },
    created(){
        axios.get(`${config.apiURL}/tarefas`)
            .then((response) => {
                console.log(response)
                this.tarefas = response.data
            })
        }
    }
</script>