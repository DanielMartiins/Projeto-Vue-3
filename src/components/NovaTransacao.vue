import { CREDITO, DEBITO } from "../variaveis-globais";

Vue.component("nova-transacao", {
  props: ["transacoes"],

  data() {
    return {
      novaTransacao: {
        descricao: "",
        valor: "",
        valido: true,
      },
    };
  },

  template: `
  <section id="nova-transacao">
    <h1>Nova transação</h1>

    <div>
      <label for="nova-transacao__descricao">Descricao</label>
      <input
        id="nova-transacao__descricao"
        type="text"
        v-model="novaTransacao.descricao"
        maxlength="34"
      />
    </div>

    <div>
      <label for="nova-transacao__valor">Valor</label>
      <input
        id="nova-transacao__valor"
        type="number"
        v-model="novaTransacao.valor"
      />
    </div>
    <span style="color: red" v-if="!novaTransacao.valido">
      Transação inválida! Verifique os dados e tente novamente
    </span>
    <button value="Adicionar Transação" @click="adicionarTransacao()">
      Adicionar Transação
    </button>
  </section>
  `,

  methods: {
    definirOperacao: function (valorTransacao) {
      if (valorTransacao >= 0) return CREDITO;
      else return DEBITO;
    },

    resetarFormulario: function () {
      this.novaTransacao.descricao = "";
      this.novaTransacao.valor = "";
      this.novaTransacao.valido = true;
    },

    adicionarTransacao: function () {
      let valorTransacao = parseFloat(this.novaTransacao.valor);
      let descricaoTransacao = this.novaTransacao.descricao;

      if (descricaoTransacao.trim() === "" || isNaN(valorTransacao))
        this.novaTransacao.valido = false;
      else {
        let objTransacao = {
          descricao: descricaoTransacao,
          valor: valorTransacao,
          operacao: this.definirOperacao(valorTransacao),
        };
        this.transacoes.push(objTransacao);
        this.resetarFormulario();
      }
    },
  },
});
