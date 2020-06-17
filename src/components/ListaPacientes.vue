<!-- ESTRUTURA -->

<template>
  <table class="container table-striped">
    <thead>
      <tr>
        <th>
          <h1>ID</h1>
        </th>
        <th>
          <h1>Nome</h1>
        </th>
        <th>
          <h1>Endereço</h1>
        </th>
        <th>
          <h1>Telefone</h1>
        </th>
        <th>
          <h1>Peso</h1>
        </th>
        <th>
          <h1>Altura</h1>
        </th>
        <th>
          <h1>Idade</h1>
        </th>
        <th>
          <h1>Complicações</h1>
        </th>
        <th>
          <h1>Obesidade</h1>
        </th>
        <th>
          <h1>Idoso</h1>
        </th>
      </tr>
    </thead>
    <tbody
      :class="{ active: index == currentIndex }"
      v-for="(paciente, index) in pacientes"
      :key="index"
    >
      <tr>
        <td class="id"><a class="tagID" :href="'pacientes/' + paciente.id" data-toggle="modal">{{ paciente.id }}</a></td>
        <td>{{ paciente.nome }}</td>
        <td style="width: 200px">{{ paciente.endereco }}</td>
        <td>{{ paciente.telefone }}</td>
        <td>{{ paciente.peso }}</td>
        <td>{{ paciente.altura }}</td>
        <td>{{ paciente.idade }}</td>
        <td>{{ paciente.prob_saude }}</td>
        <td>{{ paciente.obesidade }}</td>
        <td>{{ paciente.idoso }}</td>
      </tr>
    </tbody>
    </table>
</template>

<!-- COMPORTAMENTO -->

<script>
import PacienteWS from "../services/PacientesWS";

export default {
  name: "listarPacientes",
  data() {
    return {
      pacientes: [],
      currentIndex: -1,
      currentPaciente: null,
      nome: ""
    };
  },
  methods: {
    obterPacientes() {
      PacienteWS.getAll()
        .then(pacientes => {
          this.pacientes = pacientes.data;
          console.log(this.pacientes);
        })
        .catch(error => {
          console.log(error.message);
        });
    },
    refreshPacientes() {
      this.obterPacientes();
      this.currentPaciente = null;
      this.currentIndex = -1;
    },
    selecionarPaciente(paciente, index) {
      this.currentPaciente = paciente;
      this.currentIndex = index;
    },
    removerTodosPacientes() {
      PacienteWS.deletarTodosPacientes()
        .then(response => {
          console.log(response.data);
          this.refreshPacientes();
        })
        .catch(error => {
          console.log(error.message);
        });
    }
  },
  mounted() {
    this.obterPacientes();
  }
};
</script>

<!-- ESTILIZAÇÃO -->

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>