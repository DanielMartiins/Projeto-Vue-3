<script setup>
import { CREDITO, DEBITO } from '../utils/variaveis-globais'
import { ref } from 'vue'

const props = defineProps({
  transacoes: {
    type: Array,
    required: true,
  },
})
const novaTransacao = ref({
  descricao: '',
  valor: NaN,
  ehValido: true,
})

function definirOperacao(valorTransacao) {
  if (valorTransacao >= 0) return CREDITO
  else return DEBITO
}

function resetarFormulario() {
  novaTransacao.value.descricao = ''
  novaTransacao.value.valor = NaN
  novaTransacao.value.ehValido = true
}

function adicionarTransacao() {
  let valorTransacao = parseFloat(novaTransacao.value.valor)
  let descricaoTransacao = novaTransacao.value.descricao

  if (descricaoTransacao.trim() === '' || isNaN(valorTransacao))
    novaTransacao.value.ehValido = false
  else {
    let transacao = {
      descricao: descricaoTransacao,
      valor: valorTransacao,
      operacao: definirOperacao(valorTransacao),
    }
    props.transacoes.push(transacao)
    resetarFormulario()
  }
}

resetarFormulario()
</script>

<template>
  <div id="nova-transacao">
    <section>
      <h1>Nova transação</h1>

      <div class="campo-formulario">
        <label for="nova-transacao__descricao">Descricao</label>
        <input id="nova-transacao__descricao" type="text" v-model="novaTransacao.descricao" maxlength="34" />
      </div>

      <div class="campo-formulario">
        <label for="nova-transacao__valor">Valor</label>
        <input id="nova-transacao__valor" type="number" v-model="novaTransacao.valor" />
      </div>
      <div id="container-adicionar-transacao">
        <div id="mensagem-erro">
          <span style="color: red" v-show="!novaTransacao.ehValido">
            Transação inválida! <br>Verifique os dados e tente novamente
          </span>
        </div>
        <button value="Adicionar Transação" @click="adicionarTransacao()">Adicionar Transação</button>
      </div>
    </section>
  </div>
</template>

<style scoped lang="scss">
#nova-transacao {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(36, 41, 57);
  flex-direction: column;
  width: 100%;
  height: 100%;
  margin: 0;
  margin-top: 10px;
  padding: 0;
  font-size: 18px;

  // * {
  //   border: 1px solid red;
  // }

  section {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;

  }

  h1 {
    border-bottom: 1px solid white;
    width: 100%;
  }

  div {
    width: 60%;
  }
}

.campo-formulario {
  display: flex;
  text-align: left;

  input {
    width: 300px;
    margin-left: auto;
  }

  label {
    width: 100px;
  }
}

#container-adicionar-transacao {
  display: flex;
  justify-content: space-between;
  align-items: center;


  button {
    width: 150px;
  }

  #mensagem-erro {
    width: 250px;
    height: 50px;
    padding: 0;
    margin: 0;

    span {
      text-align: left;
      padding: 0;
      margin: 0;
    }
  }
}
</style>
