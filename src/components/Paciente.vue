<template>
    <div v-if="currentPaciente" class="edit-form">
        <h4>Paciente</h4>
        <form>
            <div class="form-group">
                <label for="nome">Nome</label>
                <input type="text" class="form-control" id="nome" v-model="currentPaciente.nome">
            </div>
            <div class="form-group">
                <label for="endereco">Endereço</label>
                <input type="text" class="form-control" id="endereco" v-model="currentPaciente.endereco">
            </div>
            <div class="form-group">
                <label for="telefone">Telefone</label>
                <input type="text" class="form-control" id="telefone" v-model="currentPaciente.telefone">
            </div>
            <div class="form-group">
                <label for="peso">Peso</label>
                <input type="number" class="form-control" id="peso" v-model="currentPaciente.peso">
            </div>
            <div class="form-group">
                <label for="altura">Altura</label>
                <input type="number" class="form-control" id="altura" v-model="currentPaciente.altura">
            </div>
            <div class="form-group">
                <label for="idade">Idade</label>
                <input type="number" class="form-control" id="idade" v-model="currentPaciente.idade">
            </div>
            <div class="form-group">
                <label for="prob_saude">Complicação</label>
                <input type="text" class="form-control" id="prob_saude" v-model="currentPaciente.prob_saude">
            </div>
        </form>
        <button class="btn badge-danger mr-2" @click="deletarPaciente">Deletar Paciente</button>
        <button type="submit" class="btn badge-success mr-2" @click="atualizarPaciente">Atualizar Paciente</button>
        
    </div>
</template>

<script>

    import PacienteWS from "../services/PacientesWS";

    export default {
        name: "paciente",
        data(){
            return {
                currentPaciente: null,
                message: ''
            }
        }, 
        methods: {
            getPaciente(id){
                PacienteWS.getPacienteID(id)
                    .then(paciente => {
                        this.currentPaciente = paciente.data;
                        let data = this.currentPaciente.data_internacao;
                        data = this.currentPaciente.data_internacao.split('T')[0];
                        console.log(data);
                        this.currentPaciente.data_internacao = data;
                    })
                    .catch(error => {
                        console.log(error);
                    });
            },
            deletarPaciente(){
                PacienteWS.deletarPacienteID(this.currentPaciente.id)
                    .then(response => {
                        console.log(response.data);
                        this.message = 'O paciente foi deletado com sucesso!'
                    })
                    .catch(error => {
                        console.log(error.message);
                    })
                window.location.replace("http://localhost:8081/pacientes")
            },
            atualizarPaciente(){
                PacienteWS.atualizarPaciente(this.currentPaciente.id, this.currentPaciente)
                    .then(response => {
                        console.log(response.data);
                        this.message = "Paciente atualizado com sucesso!"
                    })
                    .catch(error => {
                        console.log(error.message);
                    })
                window.location.replace("http://localhost:8081/pacientes")
            }
        },
        beforeMount(){
            this.message = '',
            this.getPaciente(this.$route.params.id);
        }
    }
</script>

<style>

</style>