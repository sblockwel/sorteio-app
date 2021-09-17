<template>
  <div>
    <h1>Sorteio - Vue</h1>
    <p :title="informacao">Autor: {{ autor.toUpperCase() }} - {{ criacao }} </p>
    
    <h2>Informações adicionais</h2>

    <form id="formulario" @submit.prevent="salvarParticipante">
      <div class="campo">
        <label for="nome"> Nome: </label><br />
        <input type="text" id="nome" autofocus required v-model="participante.nome" />
      </div>
      <div class="campo"> 
        <label for="email"> Email: </label>
        <input type="email" id="email" required v-model="participante.email" />
      </div>
      <div id="botao" class="campo botao">
        <input type="submit" :value="acao" />
      </div>
    </form>

    <div>
      <label>Filtrar</label><br/>
      <input type="text" id="nome" v-model="search">
    </div>

    <div >
      <table class="tabela">
        <th><a href="#" @click="ordenar('nome')">Nome</a></th>
        <th>Email</th>
        <th>Opções</th>
        
        <tr v-for="(p, idx) in filteredItems" :key="p.nome" :class="{ 'alteracao': alteracaoIdx == idx }" class="linhas">
          <td>{{ p.nome }}</td>
          <td>{{ p.email }}</td>
          <td>
            <button @click="alterar(idx)">Alterar</button>
            <button @click="remover(p)">Remover</button>
          </td>
        </tr>
      </table>
    </div>
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
      alteracaoIdx: -1,
      search: ""
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
    },
    ordenar(chave) {
      this.participantes.sort(function(a, b) {
        return a[chave].localeCompare(b[chave])
      })
    }
  },
  computed: {
    acao() {
      return this.alteracaoIdx > -1 ? "Alterar" : "Adicionar"
    },
    filteredItems() {

      let part = []

      part = this.participantes.filter(p => p = p.nome.toLowerCase().indexOf(this.search.toLowerCase()) > -1 )
        
 
      return part
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
  background-color:#d4d4d4
}

#formulario {
  width: 60%; 
  margin-left: 20%;
  padding: 45px 15px;
  height: 100px;
}

.campo {
  width: 40%;
  float: left;
}

.campo input {
  margin: 10px 1%;
  padding: 8px 1%;
  width: 90%;
}

#botao {
    width: 20%;
    height: 50px;
    margin-top: 17px;
}

.tabela {
  margin: auto;
}

table {
    border-collapse: collapse;
    width: 50%;
}

th, td {
    padding: 8px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

tr:hover{
  background-color:#d4d4d4

}

</style>
