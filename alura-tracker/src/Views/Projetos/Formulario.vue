<template>
    <section class="projetos"> 
        <router-link to="/projetos" class="button is-info">
            <span class="icon is-small">
                <i class="fas fa-arrow-left"></i>
            </span>
            <span>Voltar para listagem</span>
        </router-link>
        <form @submit.prevent="salvar">
            <div class="field">
                <label for="nomeDoProjeto" class="label">
                    Nome do Projeto
                </label>
                <input type="text" class="input" v-model="nomeDoProjeto" id="nomeDoProjeto"> 
            </div>
            <div class="field">
                <button class="button" type="submit">Salvar</button>
            </div>
        </form> 
    </section>
</template>

<script lang="ts">
    import { TipoNotificacao } from "@/interfaces/INotificacao";
    import { useStore } from "@/store";
    import { ALTERA_PROJETO, ADICIONA_PROJETO } from "@/store/tipo-mutacoes";
    import { defineComponent } from "vue";   
    import useNotificador from "@/hooks/notificador";
    export default defineComponent({
        name:"Formulario",
        props: {
            id:{
            type: String
            }
        },
        data(){
            return {
                nomeDoProjeto: ''
            };
        },
        mounted(){
            if(this.id){
                const projeto = this.store.state.projetos.find(proj => proj.id == this.id);
                this.nomeDoProjeto = projeto?.nome || ''
            }
        },
        methods:{ 
            salvar(){
                if(this.id){
                    this.store.commit(ALTERA_PROJETO,{id:this.id,nome:this.nomeDoProjeto});
                }else{
                    this.store.commit(ADICIONA_PROJETO,this.nomeDoProjeto);
                    this.nomeDoProjeto = '';
                    this.notificar(TipoNotificacao.SUCESSO,'Operação executada com sucesso !','O projeto foi salvo com sucesso !');
                     
                    this.$router.push('/projetos');
                }
                

            }
        },
        setup(){
            const store = useStore();
            const { notificar } = useNotificador();
            return { 
                store,
                notificar
            }
        }
    });

</script>