<template>
  <div class="principal">
    <h2>Converse aqui:</h2>
    <div class="conversa" >
      <div  class="linha" v-for="(linha, index) in conversa" :key="index">
        <ChatLinhaDialogo :linha="linha" />
      </div>
      <span v-show="digitando">Digitando ...</span>
    </div>
    <input
      type="text"
      id="pergunta"
      v-model="mensagem"
      name="pergunta"
      class="campo"
      placeholder="Digite sua pergunta"
    />
    <button 
     :disabled="!mensagem"
     id="enviar" 
     @click="carregarDados"
     >
      Enviar
     </button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import ChatLinhaDialogo from './ChatLinhaDialogo.vue';
import axios, { AxiosResponse } from 'axios';

@Component({
  components: {
    ChatLinhaDialogo
  },
})
export default class ChatBot extends Vue {
  @Prop() private msg!: string;

  conversa: Array<object> = []
  mensagem = ''
  sessionId = ''
  digitando = false

  async carregarDados() {
    const mensagem: string = this.mensagem;
    this.mensagem = '';
    this.digitando = true;
    this.atualizarConversa(mensagem, 'me');
    try {
      const resposta: AxiosResponse = await this.respostaDoBot(mensagem);
      const resultado = resposta.data;
      this.sessionId = resultado.session_id;
      this.atualizarConversa(resultado.respostas[0].text, 'bot')
    }catch(error) {
      console.log(error)
      this.sessionId = '';
    }finally {
      this.digitando = false;
    }
  }

  atualizarConversa(text: string, tipo: string): void {
    if( text ) this.conversa.push({ text, tipo });
  }

  async respostaDoBot(mensagem: string): Promise<AxiosResponse> {
    return axios(`http://localhost:1880/chat?mensagem=${mensagem}&session_id=${this.sessionId}`);
  }
    
  created() {
    this.carregarDados();
  }


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.principal {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
}

.campo {
	display: block;
	width: 440px;
	line-height: 20px;
	margin: 20px 0;
	padding: 10px 5px;
	font-size: 14px;
	border: 0;
	border-bottom: 2px #fff solid;
	color: #fff;
	background: #3a3a3b;
	outline: none;
}

input::placeholder {
	color: rgba(255, 255, 255, 0.5);
}

.conversa {
	width: 450px;
	height: 300px;
	background: #fff;
	color: #3a3a3b;
	font-size: 14px;
	font-family: verdana;
	overflow: auto;
	position: relative;
}

.conversa span {
	width: 90%;
	background-color: #fff;
	color: #000;
	position: fixed;
	bottom: 122px;
	left: 16px;
	transition: all 1s ease-in;
	/* display: none; */
	z-index: 10;
	padding: 10px 0;
}

span.ativo {
	display: block;
}

.linha {
	display: flex;
	flex-direction: column;
}



h2 {
	text-align: center;
	color: #fff;
	font-family: helvetica;
	text-transform: uppercase;
	font-size: 20px;
}

button {
	display: block;
	background: #428bca;
	font-size: 16px;
	color: #fff;
	padding: 10px 20px;
	border: 0;
	border-bottom: 2px #428bca solid;
	border-radius: 3px;
	position: relative;
	left: 40%;
	text-shadow: 0.05em 0.05em grey;
}

button:hover {
	background: rgba(66, 139, 202, 0.8);
	cursor: pointer;
}

</style>
