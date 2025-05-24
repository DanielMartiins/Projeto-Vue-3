<script setup>
import { ref } from 'vue';
import ListaTransacoes from './components/ListaTransacoes.vue';
import NovaTransacao from './components/NovaTransacao.vue';

const CREDITO = "Crédito: +";
const DEBITO = "Débito: -";

const transacoes = ref([
    { descricao: "Venda", valor: +6072.0, operacao: CREDITO },
    { descricao: "Conta", valor: -100.0, operacao: DEBITO },
    { descricao: "Aluguel", valor: -3000.0, operacao: DEBITO },
    { descricao: "Receita", valor: +6072.0, operacao: CREDITO },
    { descricao: "Luz", valor: -170.0, operacao: DEBITO },
    { descricao: "Salario", valor: +6072.52, operacao: CREDITO },
    { descricao: "IPVA", valor: -1200.99, operacao: DEBITO },
    { descricao: "Produto X", valor: -15000.99, operacao: DEBITO },
]);


</script>

<template>


    <main>
        <ListaTransacoes v-bind:transacoes='transacoes'></ListaTransacoes>
        <NovaTransacao v-bind:transacoes='transacoes'></NovaTransacao>
    </main>
</template>

<style scoped></style>
