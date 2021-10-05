<template>
    <div>
        <form id="formulario" >
        <div class="campo">
            <label for="numero"> Número: </label><br />
            <input type="number" id="numero" autofocus required v-model="infoPedido.numero" />
        </div>
        <div class="campo"> 
            <label for="data"> Data Emissão: </label>
            <input type="datetime-local" id="data" required v-model="infoPedido.data" />
        </div>
        <div class="campo"> 
            <label for="cliente"> Cliente: </label>
            <input type="select" id="cliente" required v-model="infoPedido.cliente" />
        </div>
        <div id="botao" class="campo ">
            <input type="submit" :value="acao" />
        </div>
        </form>
    </div>
</template>

<script>
export default {
  data() {
    return {
      infoPedido: {
        numero: null,
        data: null,
        cliente: null,
      }
    }
  }
}
</script>

<style>

</style>