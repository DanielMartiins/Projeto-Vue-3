<script setup>
import { ref } from 'vue'
import { computed } from 'vue'
import { onMounted } from 'vue';

const listaTransacoes = ref(null)

const props = defineProps({
    transacoes: {
        type: Array,
        required: true,
    }
});

function removerTransacao(index) {
    props.transacoes.splice(index, 1);
}

function trocarTransacoes(index_t1, index_t2) {
    let temp = props.transacoes[index_t1];
    props.transacoes[index_t1] = props.transacoes[index_t2];
    props.transacoes[index_t2] = temp;
}

function scrollarListaParaBaixo() {
    listaTransacoes.value.scrollTop = listaTransacoes.value.scrollHeight;
}

const saldoAposTransacao = computed(() => {
    let saldo = 0.0;
    let saldoAcumulado = [];
    for (let i = 0; i < props.transacoes.length; i++)
        saldoAcumulado.push(saldo += props.transacoes[i].valor)
    return saldoAcumulado;
})

onMounted(() => {
    scrollarListaParaBaixo();
})

</script>

<template>
    <section id="lista-transacoes">
        <h1>Lista de Transações</h1>
        <div id="legenda-transacao" >
            <div>
                <span>Descrição</span>
                <span>Transação</span>
                <span>Saldo atual</span>
            </div>
        </div>
        <ul ref="listaTransacoes" style="list-style-type: none">
            <li v-for="(t, index) in transacoes">
                <span v-text="t.descricao"></span>
                <span v-bind:class="t.valor >= 0 ? 'credito' : 'debito'"
                    v-text="t.operacao + 'R$ ' + Math.abs(t.valor).toFixed(2)"></span>
                <span v-bind:class="saldoAposTransacao[index] >= 0 ? 'saldo-positivo' : 'saldo-negativo'"
                    v-text="'R$ ' + saldoAposTransacao[index].toFixed(2)"></span>
                <span class="seta-troca-transacao">
                    <button @click="trocarTransacoes(index, index - 1)" :disabled="index == 0">&#8593</button>
                    <button @click="trocarTransacoes(index, index + 1)"
                        :disabled="index === transacoes.length - 1">&#8595;</button>
                </span>
                <button @click="removerTransacao(index)">Remover</button>
            </li>
        </ul>
    </section>
</template>


<style scoped>

button {
    margin-left: auto;
}

li,
#legenda-transacao {
    display: flex;
    justify-content: flex-start;
    overflow-wrap: break-word;
    align-items: center;
    height: 50px;
    background-color: rgb(36, 41, 57);
}

#legenda-transacao {
    outline: 1px solid;
    margin: 0px;
}

#legenda-transacao div {
    width: 100%;
    display: inherit;
}

#legenda-transacao span {
    font-size: 18px;
    font-weight: bold;
    text-decoration: underline;
}

.seta-troca-transacao button {
    width: 20px;
    font-size: 20px;
}

.credito {
    color: rgb(115, 255, 115);
}

.debito {
  color: rgb(255, 115, 115);;
}
.saldo-negativo {
    background-color: darkred;
}

.saldo-negativo {
    background-color: darkred;
    color: yellow;
    font-weight: bold;
}

.transacao-selecionada {
    border: 2px solid rgb(249, 255, 164);
    border-radius: 10px;
    margin: 5px;
}
</style>
