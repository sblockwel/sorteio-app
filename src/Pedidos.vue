<template>
    <div>
        <h1>Central de Pedidos</h1>
        <pedido-form-componente> </pedido-form-componente>
        <pedido-lista-componente></pedido-lista-componente>
    </div>
</template>

<script>
import PedidosFormComponente from 'src/components/PedidoFormComponente.vue'
import PedidosListaComponente from 'src/components/PedidoListaComponente.vue'

export default {
    components: {
        PedidosFormComponente,
        PedidosListaComponente
    }
}
</script>

<style>

</style>