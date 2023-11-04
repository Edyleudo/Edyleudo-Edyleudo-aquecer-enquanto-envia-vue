<template>
  <div id="app" class="app">
    <meta charset="utf8mb4" />

    <router-view></router-view>
    <label style="font-size: 20px">Lista de Telefones</label>
    <br />
    <textarea placeholder="Cole aqui sua lista de telefones" v-model="telefone" rows="10" cols="50" />
    <br />
    <br />
    <div class="col-md-2 col-sm-6 col-12">
        <div class="form-group">
          <label for="user_type">Selecione o turno da mensagem padrão</label>
          <select class="form-control" id="user_type" v-model="turno_selecionado" @change="chargeTurno(turno_selecionado)">
            <option v-for="turno in turnos" :key="turno" :value="turno">
              {{ turno }}
            </option>
          </select>
        </div>
      </div>
      <br />
      <br />

      <button class="button" type="button" @click="VerificarArrays">
      Baixar Planilha CSV
    </button>

  </div>
</template>
  
<!-- <script src="https://kit.fontawesome.com/your-code-number.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.17/vue.min.js"></script> -->
  
  <script>
  import writeXlsxFile from 'write-excel-file'


  // import { AtomSpinner } from "epic-spinners";
  // import sweetAlert from "../controller/sweetAlert";
  // import moment from "moment";
  
  export default {
    name: "ShowMessages",
  
    mixins: [writeXlsxFile],
  
    components: {
      // AtomSpinner,
    },
  
    data() {
      return {
        nome: [],
        telefone: [],
        turnos: ["Bom dia", "Boa tarde", "Boa noite"],
        turno_selecionado: "Bom dia",
        quantidades: [],
        qtd_lista: [],
        qtd_listas_selecionada: 1,
        novoArray : [],
        TelefonesEnviados : [],
        jsons: [],
        objJson : [],
        TelefoneTemp: [],
        percorreVetor: 0,
        //arrayBots : [5521988990638, 5521959061248, 5521959020140, 5521959090968, 5521989690938, 5521984570396, 5521985831644, 5521988850242],
        // arrayBots : [5511999910621, 5521999816059, 556140040001, 551133350237, 5511974627106, 5521967364216, 5521972275834, 5521995940372],
        arrayBots : [5521967364216, 5521972275834, 5521995940372], //ao mudar a quantidade, lembrar de mudar na função de sortear
        cont : 0,
        // frases : ["olá", "oi", "bom dia", "tudo bem?", "fala man", "e ai, contas as novas", "ei, bora sair", "como andam as coisas?"],
        frases : ["aquecimento 1", "aquecimento 2", "aquecimento 3", "aquecimento 4", "aquecimento 5", "aquecimento 6", "aquecimento 7", "aquecimento 8", "aquecimento 9", "aquecimento 10", "rei", "rainha", "princesa", "príncipe", "cavalheiro", "trono", "poltrona", "guilhotina", "blue", "yellow", "purple", "abatatado", "aborígine", "abude", "xenize", "pândego", "alarife", "despautério", "pelego", "yoni", "zodíaco", "xilogravura", "urdidura", "tergiversação", "subjugação", "vassalo", "wau", "refulgência", "sibilação", "tautologia", "escamoteação", "fescenino", "genuflexão", "heterodoxo", "inanidade", "jusante", "kafkiano", "lânguido", "macilento", "néscio", "obnóxio", "plenilúnio", "coalescer", "disrupção", "enigmático", "fustigar", "gélido", "hipnótico", "incólume", "egregóreo", "furtivo", "inóxio", "jactância", "kênosis", "labirinto", "mnemônico", "onírico", "proêmio", "quimera", "rascunho", "sapiência", "trivialidade", "ubiquidade", "visceral", "wampum", "elucidar", "emérito", "enlevo", "endógeno", "encômio", "encefalite", "ectodérmico", "camerlengo", "camelopardalis", "caprípede", "caporal", "capicua", "candelabro", "cancan", "camélio", "caliptro", "caliope", "achicar", "acongojado", "adamascado", "adâmico", "acheté", "acérrimo", "barbicacho", "barafunda", "balbúrdia", "bagual", "bacurau", "baciada", "bacanérrimo", "bacamarte", "babelismo", "babado"],
        Mensagens : [" Como vai?", " Tudo bem?", " Como vão as coisas?", " Como você está?"],
        Saldacao: ["Oi ", "Olá "]


        
      };
    },
  
    mounted() {
      this.quantidades = [];
      this.quantidades.push({ quantidade: 0 });
      
    },
  
    methods: {

    // changeListas() {
    //   console.log("CHANGE LISTAS " + this.qtd_listas_selecionada);

    //   this.quantidades = [];

    //   for (let i = 0; i < this.qtd_listas_selecionada; i++) {
    //     this.quantidades.push({ quantidade: 0 });
    //   }
    // },

    // chargeMensagem(mensagem){
    //     this.mensagem_funil_selecionada = mensagem;
    // },

    VerificarArrays(){
      this.TelefonesEnviados = this.telefone.split("\n")

      this.qtd_lista = this.TelefonesEnviados.length;

        for(let j = 0; j < this.qtd_lista; j++){
          this.TelefoneTemp.push(this.TelefonesEnviados[this.percorreVetor + j])
        }
        this.percorreVetor = this.percorreVetor + this.qtd_lista
        this.receberLista();     
      
    },


    receberLista(){
      for (let i = 0; i < this.TelefoneTemp.length; i++) {
          let telefone = [];
          let numeros_sorteados = [];
          if (this.cont == 1) {
            numeros_sorteados = this.gerarNumerosDiferentes();
              for (let j = 0; j < 3; j++) {
                  telefone = this.arrayBots[numeros_sorteados[j]].toString().substr(2);
                  // console.log(numeros_sorteados);

                  let Json = {
                      Telefone: telefone,
                      mensagem: this.sorteiaFrases()
                  }
              
                  this.novoArray.push(Json);
              }
              this.cont = 0;
              i--;
          }
          else {
              telefone = this.TelefoneTemp[i].toString();
              this.cont++;
                let Json = {
                  Telefone: telefone,
                  mensagem: this.sorteiaSaldacao() + this.turno_selecionado + this.sorteiaMensagens(),
                }
                this.novoArray.push(Json);
          }
        }
        
        // this.quantidades[posicao].quantidade = JSON.stringify(this.novoArray);
        this.objJson = JSON.stringify(this.novoArray);
        this.TelefoneTemp= [];
        this.novoArray= [];
        this.objJson = JSON.parse(this.objJson);
        this.exportDataToCsv();

      },

    sorteiaFrases(){
      return (this.frases[Math.floor(Math.random() * this.frases.length)])
    },

    sorteiaMensagens(){
      return (this.Mensagens[Math.floor(Math.random() * this.Mensagens.length)])
    },

    sorteiaSaldacao(){
      return (this.Saldacao[Math.floor(Math.random() * this.Saldacao.length)])
    },

    chargeTurno(turno){
        this.turno_selecionado = turno;
      },

    copiar(texto){
      let textoCopiado = texto;
      textoCopiado.select();
      document.execCommand("copy");
    },

    gerarNumerosDiferentes() {
      var numeros = [];
      
      while (numeros.length < 3) {
        var numeroAleatorio = Math.floor(Math.random() * 3); // Gera um número entre 0 e 2
        if (!numeros.includes(numeroAleatorio)) {
          numeros.push(numeroAleatorio);
        }
      }
      
      return numeros;
    },


      exportDataToCsv (){
        var legacy = require('legacy-encoding');
        var csvContent = this.convertToCsv(this.objJson);
        var buffer = legacy.encode(csvContent, 'windows-1252');
        
        console.log(typeof csvContent);
        const blob = new Blob([buffer], {type: 'text/csv;charset=windows-1252', endings: "native"});
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.setAttribute('download', 'export_data.csv');
        link.click();
      },

      convertToCsv(data){
        const headers = Object.keys(data[0]);
        const rows = data.map(obj => headers.map(header => obj[header]));
        const headerRow = headers.join(';');
        const csvRows = [headerRow, ...rows.map(row => row.join(';'))];
        return csvRows.join('\n')
      },

    },
  };
  </script>
  
<style scoped>
.float {
  position: fixed;
  width: 80px;
  height: 80px;
  bottom: 10px;
  right: 40px;
  background-color: #25d366;
  color: #FFF;
  border-radius: 50px;
  text-align: center;
  font-size: 12px;
  box-shadow: 2px 2px 3px #999;
  z-index: 100;
}

.my-float {
  margin-top: 16px;
}

form {
  display: grid;
  padding: 1em;
  background: #f9f9f9;
  border: 1px solid #c1c1c1;
  margin: 2rem auto 0 auto;
  max-width: 500px;
  padding: 1em;
}

form input {
  background: #fff;
  border: 1px solid #9c9c9c;
}

form button {
  background-color: blue;
  color: white;
  font-size: 1em;
  font-weight: bold;
  padding: 0.7em;
  width: 100%;
  border: 0;
}

form button:hover {
  background: gold;
  color: black;
}

label {
  padding: 0.5em 0.5em 0.5em 0;
}

input {
  padding: 0.7em;
  margin-bottom: 0.5rem;
}

input:focus {
  outline: 3px solid gold;
}

@media (min-width: 400px) {
  form {
    grid-template-columns: 150px 1fr;
    grid-gap: 16px;
  }

  label {
    text-align: right;
    grid-column: 1 / 2;
  }

  input,
  button {
    grid-column: 2 / 3;
  }
}

button {
  background-color: blue;
  color: white;
  font-weight: bold;
  border-radius: 10px;
}

button:focus {
  outline: none;
}

form button:disabled,
form button[disabled] {
  border: 1px solid #999999;
  background-color: #cccccc;
  color: #666666;
}

section {
  margin: 10px 0;
}

img {
  max-width: 300px;
  height: auto;
}

.fa {
  margin: 2px 4px;
}

.table {
  font-size: 13px;
}

.cursor {
  cursor: pointer;
}

.button-voltar {
  background-color: #b8ab39;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}

.button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  width: 100%;
  max-width: 300px;
}

.buttonCopy {
  background-color: #0d4fdd;
  border: none;
  color: white;
  padding: 6px 25px;
  text-align: center;
  text-decoration: none;
  font-size: 10px;
  margin: 4px 2px;
  cursor: pointer;
  width: 80px;
  max-width: 150px;
  border-radius: 10px;
}

.app {
  max-width: 900px;
  margin-right: auto;
  margin-left: auto;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.button-exit {
  background-color: #a67e21;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}

table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 20px;
}
</style>
  