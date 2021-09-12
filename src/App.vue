<template>
  <div>
    <h1>Sorteio - Vue</h1>
    <p :title="informacao">Autor: {{ autor.toUpperCase() }} - {{ criacao }} </p>
    
    <h2>Informações adicionais</h2>

    <form @submit.prevent="salvarParticipante">
      <label for="nome"> Nome: </label><br />
      <input type="text" id="nome" autofocus required v-model="participante.nome" />
      <br />
      <label for="email"> Email: </label><br />
      <input type="email" id="email" required v-model="participante.email" />
      <br /><br />
      <input type="submit" :value="acao" />
    </form>

    <table>
      <tr>
        <th>Nome</th>
        <th>Email</th>
        <th></th>
      </tr>
      <tr v-for="(p, idx) in participantes" :key="p.nome" :class="{ 'alteracao': alteracaoIdx == idx }">
        <td>{{ p.nome }}</td>
        <td>{{ p.email }}</td>
        <td>
          <button @click="alterar(idx)">Alterar</button>
          <button @click="remover(p)">Remover</button>
        </td>
      </tr>
    </table>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      autor: "Sabrina Blockwel",
      criacao: "criando em " + new Date().toLocaleString(),
      informacao: 'Posso colocar uma informação customizada',
      participantes: [
        { nome: "Fulano da Silva", email: "fulano@gmail.com" },
        { nome: "Cicrano de Souza", email: "cicrano@gmail.com" },
        { nome: "Beltrano Oliveira", email: "beltrano@gmail.com" },
      ],
      participante: {
        nome: null,
        email: null
      },
      alteracaoIdx: -1
    }
  },
  methods: {
    salvarParticipante() {
      const participanteSalvar = Object.assign({}, this.participante)
      if (this.alteracaoIdx > -1) {
        this.participantes[this.alteracaoIdx] = participanteSalvar;
      } else {
        this.participantes.push(participanteSalvar)
      }
      this.reset()
    },
    reset() {
      this.participante.nome = null
      this.participante.email = null
      this.alteracaoIdx = -1
    },
    remover(participanteParaRemover) {
      if (confirm("Excluir registro?")) {
        this.participantes = this.participantes.filter(p => p != participanteParaRemover)
        this.reset()
      }
    },
    alterar(idx) {
      this.participante = Object.assign({}, this.participantes[idx])
      this.alteracaoIdx = idx
    }
  },
  computed: {
    acao() {
      return this.alteracaoIdx > -1 ? "Alterar" : "Adicionar"
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.alteracao {
  background-color: yellow;
}
</style>
