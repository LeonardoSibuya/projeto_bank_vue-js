<script setup>
import { reactive } from 'vue';

const estado = reactive({
  saldo: 0,
  saldoTemp: 0,
  transferencia: '',
  nome: '',
  recebedor: '',
  enviadoComSucesso: false,
  enviadoComErro: false,
})

const recarga = () => {
  if (estado.saldo >= 1) {
    return estado.saldo
  } else {
    alert ('Recarga inválida, valor mínimo de R$ 1,00')
  }
}

const validaForm = () => {
  if (estado.transferencia <= estado.saldo) {
    estado.enviadoComSucesso = true;
    estado.enviadoComErro = false;
  } else {
    estado.enviadoComSucesso = false;
    estado.enviadoComErro = true;
  }
}

const msgSucesso = () => {
  return `Olá ${estado.nome}! Sua transferência para ${estado.recebedor}, valor de ${estado.transferencia} foi realizada com sucesso!`
}

const msgErro = () => {
  if (estado.transferencia > estado.saldo) {
    return 'Saldo insuficiente'
  }
}
</script>

<template>
  <div class="container">
    <header class="header">
      <h1 class="header__title">
        Fun Bank
      </h1>
    </header>
    <nav class="nav">
      <p class="nav__saldo">
        Meu saldo: R$ {{ estado.saldo || estado.saldoTemp }},00
      </p>
      <label for="recarga-saldo" class="nav__label">Recarregar saldo: </label>
      <input type="number" id="recarga-saldo" class="nav__input" @change="evento => estado.saldo = evento.target.value">
      <button type="submit" class="botao-recarga" @submit.prevent="recarga">Recarregar +</button>
    </nav>
    <section class="content">
      <div class="content__image">
        <img src="./components/images/image-section.png" alt="" class="content__image__img">
      </div>
      <div class="content__info">
        <form @submit.prevent="validaForm" class="transferir">
          <input type="text" id="nome" placeholder="Seu nome:" required class="transferir__input" v-model="estado.nome">
          <input type="text" id="recebedor" placeholder="Nome do recebedor:" required class="transferir__input" v-model="estado.recebedor">
          <input type="number" id="valor" placeholder="Valor da transferência:" required class="transferir__input" @change="evento => estado.transferencia = evento.target.value">
          <button type="submit" class="botao-transferir" @submit="validaForm"> Transferir</button>
        </form>
        <div class="content__image">
          <img src="./components/images/gif-feliz.gif" alt="" class="content__image__img-dinamic" v-if="estado.enviadoComSucesso">
          <img src="./components/images/gif-triste.gif" alt="" class="content__image__img-dinamic"  v-if="estado.enviadoComErro">
          <img src="./components/images/gif-pensativo.gif" alt="" class="content__image__img-dinamic" v-if="!estado.enviadoComSucesso && !estado.enviadoComErro">
        </div>
        <p class="msg-sucesso"  v-if="estado.enviadoComSucesso">
          {{ msgSucesso() }}
        </p>
        <p class="msg-erro" v-else="estado.enviadoComErro">
          {{ msgErro() }}
        </p>
      </div>
    </section>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

.container {
  max-width: 1024px;
  width: 100%;
  margin: 0 auto;
  padding: 0;
}

.header {
  text-align: center;
  background-image: linear-gradient(to bottom,#b954f3, #b954f3a2);
  border-radius: 36px 36px 0px 0px;
}

.header__title {
  font-size: 44px;
  padding: 20px 0;
  color: #ffffff;
  letter-spacing: 2px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-weight: bolder;
}
.nav {
  background-image: linear-gradient(to top,#73b7f7a6, #b954f3a2);
  padding-top: 36px;
  padding-right: 36px;
  display: flex;
  align-items: center;
  justify-content: end;
}

.nav__saldo {
  margin-right: 24px;
  font-size: 18px;
  font-weight: bold;
}

.nav__label {
  font-size: 18px;
  font-weight: bold;
  margin-right: 6px;
}

.nav__input {
  border: none;
  border-bottom: 1.5px solid #da47ff;
  box-shadow: 0px 1px 2px #da47ff94;
  outline: none;
  background-color: transparent;
  margin-right: 16px;
  max-width: 80px;
  color: #000;
  font-size: 14px;
}

.botao-recarga {
  padding: 6px 10px;
  font-weight: bold;
  border-radius: 6px;
  border: none;
  background-color: #da47ff;
  letter-spacing: 1px;
  cursor: pointer;
  transition: .5s ease;
}

.botao-recarga:hover {
  color: #fff;
  background-color: #cc24f7;
}
  
.content {
  background-image: linear-gradient(to bottom,#73b7f7a6, #b954f3a2);
  display: flex;
  justify-content: space-around;
  align-items: start;
  padding-bottom: 36px;
  border-radius: 0px 0px 36px 36px;
}

.content__image__img {
  max-width: 400px;
  width: 100%;
}

.content__info {
  display: block;
  text-align: center;
  margin: 0 auto;
  margin-top: 36px;
}

.transferir {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  max-width: 250px;
}

.transferir__input {
  text-align: center;
  padding: 8px;
  margin: 6px 0;
  background-color: #ffffffc7;
  border: none;
  outline: none;
  border-radius: 16px;
  letter-spacing: 1px;
  width: 250px;
}

.botao-transferir {
  font-weight: bold;
  font-size: 16px;
  padding: 8px;
  margin-top: 16px;
  background-color: #da47ff;
  border: none;
  outline: none;
  border-radius: 16px;
  letter-spacing: 1px;
  cursor: pointer;
  transition: .5s ease;
  width: 250px;
  width: 100%;
}

.botao-transferir:hover {
  color: #fff;
  background-color: #cc24f7;
  letter-spacing: 1.5px;
}

.content__image__img-dinamic {
  margin-top: 16px;
  max-width: 250px;
  max-height: 180px;
  width: 100%;
}

.msg-sucesso {
  font-size: 16px;
  font-weight: bold;
  width: 250px;
  color: #23a10a;
  text-shadow: 0px 0px 1px #00000093;
}

.msg-erro {
  font-size: 16px;
  font-weight: bold;
  width: 250px;
  color: #ff0000;
  text-shadow: 0px 0px 1px #00000093;
}
</style>
