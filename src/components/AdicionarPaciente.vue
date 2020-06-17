<template>
    <div class="submit-form">
        <div class="form-group">
            <label for="nome">Nome</label>
            <input 
                type="text" 
                class="form-control" 
                id="nome" 
                v-model="paciente.nome" 
                name="nome"
            />
        </div>
        <div class="form-group">
            <label for="endereco">Endereço</label>
            <input 
                type="text" 
                class="form-control" 
                id="endereco" 
                v-model="paciente.endereco" 
                name="endereco"
            />
        </div>
        <div class="form-group">
            <label for="telefone">Telefone</label>
            <input 
                type="text" 
                max="11" 
                class="form-control" 
                id="telefone"
                v-model="paciente.telefone" 
                name="telefone"
            />
        </div>
        <div class="form-group">
            <label for="peso">Peso</label>
            <input 
                type="number" 
                class="form-control" 
                id="peso" 
                v-model="paciente.peso" 
                name="peso"
            />
        </div>
        <div class="form-group">
            <label for="altura">Altura</label>
            <input 
                type="number" 
                class="form-control" 
                id="altura" 
                v-model="paciente.altura" 
                name="altura"
            />
        </div>
        <div class="form-group">
            <label for="idade">Idade</label>
            <input 
                type="number" 
                class="form-control" 
                id="idade" 
                v-model="paciente.idade" 
                name="idade"
            />
        </div>
        <div class="form-group">
            <label for="prob_saude">Complicações</label>
            <input 
                type="text" 
                class="form-control" 
                id="prob_saude" 
                v-model="paciente.prob_saude" 
                name="prob_saude"
            />
        </div>

        <button @click="inserirPaciente" class="btn btn-success">
            inserir Paciente
        </button>
    </div>
</template>

<script>

import PacientesWS from '../services/PacientesWS';
    export default {
        name: "adicionarPaciente",
        data(){
            return {
                paciente: {
                    id: "",
                    nome: "",
                    endereco: "",
                    telefone: "",
                    peso: 0,
                    altura: 0,
                    idade: 0,
                    prob_saude: ""
                }
            }
        },
        methods: {
            inserirPaciente(){
                let verifObeso = ""
                let verifIdoso = ""
                let IMC = this.paciente.peso / (this.paciente.altura * 2)
                
                if(IMC <= 25) verifObeso = "Não"
                else verifObeso = "Sim"

                if(this.paciente.idade >= 55) verifIdoso = "Sim"
                else verifIdoso = "Não"

                let dadosPaciente = {
                    nome: this.paciente.nome,
                    endereco: this.paciente.endereco,
                    telefone: this.paciente.telefone,
                    peso: this.paciente.peso,
                    altura: this.paciente.altura,
                    idade: this.paciente.idade,
                    prob_saude: this.paciente.prob_saude,
                    obesidade: verifObeso,
                    idoso: verifIdoso
                }
                window.location.replace("http://localhost:8081/pacientes")
                location.reload()

                PacientesWS.criarPaciente(dadosPaciente)
                    .then(resp => {
                        this.paciente.id = resp.data.id;
                        console.log(this.paciente);
                    }).catch(error => {
                        console.log(error.message);
                    })
            }
        }
    }
</script>