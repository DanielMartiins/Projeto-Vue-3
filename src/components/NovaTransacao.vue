<script setup>
import { CREDITO, DEBITO } from "../utils/variaveis-globais";
import { ref } from "vue";



const props = defineProps({
    transacoes: {
        type: Array,
        required: true,
    }
})
const novaTransacao = ref({
    descricao: String,
    valor: Number,
    ehValido: Boolean,
})
resetarFormulario();



function definirOperacao(valorTransacao) {
    if (valorTransacao >= 0) return CREDITO;
    else return DEBITO;
}

function resetarFormulario() {
    novaTransacao.value.descricao = "";
    novaTransacao.value.valor = "";
    novaTransacao.value.ehValido = true;
}

function adicionarTransacao() {
    let valorTransacao = parseFloat(novaTransacao.value.valor);
    let descricaoTransacao = novaTransacao.value.descricao;

    if (descricaoTransacao.trim() === "" || isNaN(valorTransacao))
        novaTransacao.value.ehValido = false;
    else {
        let transacao = {
            descricao: descricaoTransacao,
            valor: valorTransacao,
            operacao: definirOperacao(valorTransacao),
        }
        props.transacoes.push(transacao);
        resetarFormulario();
    }
}
</script>

<template>
    <section id="nova-transacao">
        <h1>Nova transação</h1>

        <div>
            <label for="nova-transacao__descricao">Descricao</label>
            <input id="nova-transacao__descricao" type="text" v-model="novaTransacao.descricao" maxlength="34" />
        </div>

        <div>
            <label for="nova-transacao__valor">Valor</label>
            <input id="nova-transacao__valor" type="number" v-model="novaTransacao.valor" />
        </div>
        <span style="color: red" v-if="!novaTransacao.ehValido">
            Transação inválida! Verifique os dados e tente novamente
        </span>
        <button value="Adicionar Transação" @click="adicionarTransacao()">
            Adicionar Transação
        </button>
    </section>
</template>

<style scoped>
#nova-transacao {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    font-size: 18px;
    background-color: rgb(36, 41, 57);
}

h1 {
    border-bottom: 1px solid white;
    width: 100%;
    margin: 0px;
}

#nova-transacao input,
#nova-transacao button {
    width: 300px;
}
</style>
