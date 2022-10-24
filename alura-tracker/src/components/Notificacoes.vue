<template>
    <div class="notificacoes">
        <article class="message is-success">
            <div class="message-header">Sucesso !</div>
            <div class="message-body">Sua operação foi executada com sucesso !</div>
        </article>
        <article class="message is-warning">
            <div class="message-header">Sucesso !</div>
            <div class="message-body">Sua operação foi executada com sucesso !</div>
        </article>
        <article class="message is-danger">
            <div class="message-header">Sucesso !</div>
            <div class="message-body">Sua operação foi executada com sucesso !</div>
        </article>
    </div>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';

    export default defineComponent({
        name:'Notificacoes'
    })
</script>

<style scoped>
    .notificacoes{
        position: absolute;
        right: 0;
        width: 400px;
        padding: 8px;
        z-index: 10;
    }
</style>