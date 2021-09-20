<template>
  <div>
    <h1>App de Sorteio em VueJS</h1>
      
    <div class="qtdeParticipantes">
      <label>Qtde de participantes: </label>
      <input type="number" v-model.number = "qtdeParticipantes" @change="changeInput"/>
    </div>

    <form id="formulario" v-if="ativarCampos" @submit.prevent = "salvarParticipante()" >
      <div class="campo">
        <label for="nome"> Nome: </label><br />
        <input type="text" id="nome" autofocus required v-model="participante.nome" />
      </div>
      <div class="campo"> 
        <label for="email"> Email: </label>
        <input type="email" id="email" required v-model="participante.email" />
      </div>
      <div id="botao" class="campo ">
        <input type="submit" :value="acao" />
      </div>
    </form>
    <p v-else>A quantidade de participantes foi atingida!!!</p>
   
    <div class="ajuste">
      <div class="filtro">
        <label>Filtro por nome</label><br/>
        <input type="text" v-model="search">
      </div>

      <div class="sortear">
        <button id="botao" @click="sortearParticipante()"> Sortear</button>
      </div>
    </div>

    <div >
      <table class="tabela">
        <th @click="ordenar('nome')" class="ordenar">Nome <font-awesome-icon :icon="getTipoOrdenacao('nome')" /> </th>
        <th @click="ordenar('email')" class="ordenar">Email <font-awesome-icon :icon="getTipoOrdenacao('email')" /> </th>
        <th>Opções</th>

        <tr v-for="(p, idx) in filteredItems" :key="p.nome" :class="{ 'alteracao': alteracaoIdx == idx }">
          <td id='nome' :class="{'destacar' :participanteSorteado(p.nome)}">{{p.nome}}</td>
          <td id='email' :class="{'destacar' :participanteSorteado(p.nome)}">{{p.email}}</td>
          <td>
            <button @click="alterar(idx)"><font-awesome-icon icon="pencil-alt" /></button>
            <button @click="remover(p)"> <font-awesome-icon icon="trash" /> </button>
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
      participantes: [
        { nome: "Fulano da Silva", email: "fulano@gmail.com" },
        { nome: "Cicrano de Souza", email: "cicrano@gmail.com" },
        { nome: "Beltrano Oliveira", email: "beltrano@gmail.com" }
      ],
      participante: {
        nome: null,
        email: null
      },
      alteracaoIdx: -1,
      search: "", 
      ordemCampos: {
        nome: null,
        tipo: 'asc'
      },
      qtdeParticipantes: '',
      nomeSorteado: '',
      ativarCampos: true
    }
  },
  methods: {
    salvarParticipante() {
      const participanteSalvar = Object.assign({}, this.participante)
      const existe = this.participantes.some(p => p.nome == participanteSalvar.nome)

      if (this.alteracaoIdx > -1) {
          this.participantes[this.alteracaoIdx] = participanteSalvar
      } else {
        if (existe == true) {
          alert('Essa pessoa já foi adicionada.')          
        } else {
          this.participantes.push(participanteSalvar)        
        }        
      }
      this.changeInput()
      this.reset()      
    },
    reset() {
      this.participante.nome = null
      this.participante.email = null
      this.alteracaoIdx = -1
    },
    changeInput(){
      this.ativarCampos = this.participantes.length < this.qtdeParticipantes
    },
    remover(participanteParaRemover) {
      if (confirm("Excluir registro?")) {
        this.participantes = this.participantes.filter(p => p != participanteParaRemover)
        this.reset()
      }
      this.changeInput()
    },
    alterar(idx) {
      this.participante = Object.assign({}, this.participantes[idx])
      this.alteracaoIdx = idx
      this.ativarCampos = true
    },
    sortearParticipante() {
      this.nomeSorteado = this.participantes[Math.round((Math.random() * this.qtdeParticipantes))].nome;
    }, 
    participanteSorteado(nome) {
      return this.nomeSorteado === nome
    },   
    getTipoOrdenacao(campo){
      if (campo == this.ordemCampos.nome) {
        if (this.ordemCampos.tipo == 'asc')
          return 'sort-up'
        return 'sort-down'
      }
      return 'sort'
    },
    ordenar(campo) {      
      if(campo == this.ordemCampos.nome) {
        this.ordemCampos.tipo = this.ordemCampos.tipo == 'asc' ? 'desc' :  'asc'
      } 
      this.ordemCampos.nome = campo
      const tipoOrdem = this.ordemCampos.tipo == 'asc' ? 1 : -1

      this.participantes = this.participantes.sort((a, b) =>
        a[campo].localeCompare(b[campo]) * tipoOrdem
      )      
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

button:hover{
  cursor: pointer;
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

.ordenar{
  cursor: pointer;
}

.qtdeParticipantes {
  padding: 20px;
  width: 20%;
  margin-left: 37%;
  height: 10%;
}

.qtdeParticipantes input {
  width: 10%;
  padding: 5px;
}

.destacar {
  background-color: rgb(70, 155, 184);
}

p {
  color: red;
  font-size: 40px;
  font: bolder;
}

.ajuste{
  width: 60%; 
  margin-left: 20%;
  height: 50px;
}

.filtro {
  padding-left: 100px;
  margin-bottom: 0px;
  text-align: left;
  width: 40%;
  float: left;
}

.filtro input {
  width: 50%;
}

.sortear {
  width: 30%;
  float: left;
  padding-left: 200px;
}

.sortear #botao{
  width: 80%;
  font-size: 20px;
  background-color: seagreen;
  border-radius: 20px;
  border: 0px;
}

.sortear #botao:hover{
   background-color: rgb(25, 77, 47);
   color: white;
}


</style>
