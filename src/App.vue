<script setup>
import { ref } from 'vue'
import ListaTransacoes from './components/ListaTransacoes.vue'
import NovaTransacao from './components/NovaTransacao.vue'
import { CREDITO, DEBITO } from './utils/variaveis-globais'

const transacoes = ref([
  { descricao: 'Venda', valor: +6072.0, operacao: CREDITO },
  { descricao: 'Conta', valor: -100.0, operacao: DEBITO },
  { descricao: 'Aluguel', valor: -3000.0, operacao: DEBITO },
  { descricao: 'Receita', valor: +6072.0, operacao: CREDITO },
  { descricao: 'Luz', valor: -170.0, operacao: DEBITO },
  { descricao: 'Salario', valor: +6072.52, operacao: CREDITO },
  { descricao: 'IPVA', valor: -1200.99, operacao: DEBITO },
  { descricao: 'Produto X', valor: -15000.99, operacao: DEBITO },
])

function removerTransacao(index) {
  transacoes.value.splice(index, 1)
}

function trocarTransacoes(index_t1, index_t2) {
  let temp = transacoes.value[index_t1]
  transacoes.value[index_t1] = transacoes.value[index_t2]
  transacoes.value[index_t2] = temp
}

function adicionarTransacao(novaTransacao) {
  transacoes.value.push(novaTransacao)
}

</script>

<template>
  <main>
    <ListaTransacoes
      v-bind:transacoes="transacoes"
      @remover-transacao="removerTransacao"
      @trocar-transacoes="trocarTransacoes"
    >
    </ListaTransacoes>

    <NovaTransacao @adicionar-transacao="adicionarTransacao"> </NovaTransacao>
  </main>
</template>

<style scoped></style>
