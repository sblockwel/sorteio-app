<template>
    <div>
        <div >
            <table class="tabela">
                <th @click="ordenar('produto')" class="ordenar">Produto </th>
                <th @click="ordenar('quantidade')" class="ordenar">Quantidade </th>
                <th @click="ordenar('unidade')" class="ordenar">Valor unitário </th>
                <th @click="ordenar('desconto')" class="ordenar">Valor desconto </th>
                <th @click="ordenar('total')" class="ordenar">Total </th>
                <th>Opções</th>

                <tr v-for="(p, idx) in filteredItems" :key="p.produto" :class="{ 'alteracao': alteracaoIdx == idx }">
                <td id='produto' >{{p.produto}}</td>
                <td id='quantidade' >{{p.quantidade}}</td>
                <td id='unidade' >{{p.unidade}}</td>
                <td id='desconto' >{{p.desconto}}</td>
                <td id='total' >{{p.total}}</td>
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
      pedidos: [
        { produto: "Produto 1", quantidade: "5", unidade: "2.99", desconto: "0.50", total: "" },
        { produto: "Produto 2", quantidade: "2", unidade: "7.00", desconto: "0.90", total: "" },
        { produto: "Produto 3", quantidade: "7", unidade: "1.99", desconto: "0.00", total: "" }
      ],
      pedido: {
          produto: null,
          quantidade: null,
          unidade: null,
          desconto: null,
          total: null
      },      
      alteracaoIdx: -1,
      ordemCampos: {
        nome: null,
        tipo: 'asc'
      },
      qtdePedidos: ''
    }
  }
}
</script>

<style>

</style>