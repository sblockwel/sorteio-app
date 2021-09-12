<template>
  <div>
    <h1>Sorteio - Vue</h1>
    <p :title="informacao">Autor: {{autor.toUpperCase()}} - {{criacao}}</p>
     
    <button @click="trocarInformacoes()">Mostrar/Esconder informações</button> 
    <div v-show="mostrarInformacoesComplementares">
        <h2>Informações adicionais</h2>

        <form @submit.prevent="adicionarProjeto">
          <label for="nome"> Nome: </label><br/>
          <input type="text" id="nome" autofocus required v-model="participante.nome"> <br/>
          <label for="email"> Email: </label><br/>
          <input type="email" id="email" required v-model="participante.email"> <br/><br/>
          <input type="submit" value="Adicionar projeto" > <br/>
        </form>

        <ul>
           <li v-for="p in participantes" :key="p.nome">{{p.nome}} <button @click="remover(p)"> Remover</button> <button @click="alterar(p)"> Alterar</button> </li>
        </ul>

    </div> 
  </div>
</template>

<script>

export default {
  data() {
    return {
      autor: 'Sabrina Blockwel',
      criacao: 'criando em ' + new Date().toLocaleString(),
      mostrarInformacoesComplementares: false,
      participantes: [
        {nome: "Fulano da Silva", email: 100 },
        {nome: "Cicrano de Souza", email: 80},
        {nome: "Beltrano Oliveira", email: 200}
      ],
      participante: {
        nome: null,
        email: 0
      },
      isEdit: false,
    }
  },
  methods: {
    trocarInformacoes() {
      this.mostrarInformacoesComplementares = !this.mostrarInformacoesComplementares;
    },
    adicionarProjeto(){
      /* --- primeira forma
      this.projetos.push(
        {
          nome : this.projeto.nome,
          tamanho : this.projeto.tamanho
        }
      )
      this.projeto.nome = null
      this.projeto.tamanho = 0
      */

      /* --- segunda forma*/
      const projetoAdicionar = Object.assign({}, this.projeto)
      this.projetos.push(projetoAdicionar)

      this.projeto.nome = null
      this.projeto.tamanho = 0
      
    },
    remover(projetoParaRemover){
      if(confirm("Excluir registro?")) {
        /* ---Primeira forma
        this.projetos.splice(this.projetos.indexOf(projetoParaRemover), 1)
        */

        /* --- Segunda forma*/
        this.projetos = this.projetos.filter(p => p != projetoParaRemover)
      } 
    },
    alterar(projetoParaAlterar){
      this.projetos = projetoParaAlterar;
      this.isEdit = true;

      /*exemplo da net
      let projetos = this.contacts.map(contactMap => {
                        if(contactMap.id == contact.id) {
                            return contact;
                        }

                        return contactMap;
                    });

                    this.contacts = contacts;
                    this.isEdit = false;

                    localStorage.setItem('contactsApp', JSON.stringify(contacts));

                    location.reload();*/
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
</style>
