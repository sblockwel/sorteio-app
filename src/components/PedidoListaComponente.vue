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
        { numero: "", data: "", cliente: "" },
        { numero: "", data: "", cliente: "" },
        { numero: "", data: "", cliente: "" }
      ],
      
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