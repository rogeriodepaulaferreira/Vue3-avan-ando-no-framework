<template>
  <div class="box formulario">
    <div class="columns">
      <div
        class="column is-5"
        role="form"
        aria-label="Formulário para criação de uma nova tarefa"
      >
        <input
          type="text"
          class="input"
          placeholder="Qual tarefa você deseja iniciar?"
          v-model="descricao"
        />
      </div>
      <div class="column is-3">
        <div class="select">
          <select v-model="idProjeto">
            <option value="">Selecione o projeto</option>
            <option
              :value="projeto.id"
              v-for="projeto in projetos"
              :key="projeto.id"
            >
              {{ projeto.nome }}
            </option>
          </select>
        </div>
      </div>
      <div class="column">
        <Temporizador @aoTemporizadorFinalizado="finalizarTarefa"/>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { TipoNotificacao } from "@/interfaces/INotificacao";
import { defineComponent, computed } from "vue";
import { useStore } from "@/store";
import Temporizador from './Temporizador.vue'
import { notificacaoMixin } from "@/mixins/notificar";

export default defineComponent({
  name: "Formulário",
  mixins:[notificacaoMixin],
  emits: ['aoSalvarTarefa'],
  components: {
    Temporizador
  },
  data () {
    return { 
      descricao: '',
      idProjeto: ''
    }
  },
  methods: {
    finalizarTarefa (tempoDecorrido: number) : void {
      const projeto = this.projetos.find((p) => p.id == this.idProjeto); // primeiro, buscamos pelo projeto
      if(!projeto) { 
          this.notificar(TipoNotificacao.FALHA,'Falha na tentativa de cadastro','Selecione um projeto antes de finalizar a tarefa!'); // notificamos o usuário
          return; // ao fazer return aqui, o restante do método salvarTarefa não será executado. chamamos essa técnica de early return :)
      }
      this.$emit('aoSalvarTarefa', {
        duracaoEmSegundos: tempoDecorrido,
        descricao: this.descricao,
        projeto: this.projetos.find(proj => proj.id == this.idProjeto)
      })
      this.descricao = ''
    }
  },
  setup(){
    const store = useStore();
    return {
      projetos: computed(()=> store.state.projetos),
      store
    }
  }
});
</script>
<style>
.formulario {
  color: var(--texto-primario);
  background-color: var(--bg-primario);
}
</style>