<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-sheet color="grey lighten-4" class="pa-4">
        <v-avatar class="mb-4" color="grey darken-1"></v-avatar>
      </v-sheet>

      <v-divider></v-divider>

      <v-list>
        <v-list-item v-for="[icon, text] in links" :key="icon" link>
          <v-list-item-icon>
            <v-icon>{{ icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="bg-grey">
      <v-container>
        <v-form @submit.prevent="registrarMomento">
          <v-card outlined>
            <v-card-text>
              <v-text-field
                label="Humor nome"
                name="humorNome"
                v-model="formularioMomento.humor.nome"
              ></v-text-field>
              <v-text-field
                label="Humor cor"
                name="humorCor"
                v-model="formularioMomento.humor.cor"
              ></v-text-field>
              <v-text-field
                label="Humor icone"
                name="humorIcone"
                v-model="formularioMomento.humor.icone"
              ></v-text-field>
              <v-text-field
                label="Atividade nome"
                name="atividadeNome"
                v-model="formularioMomento.atividade.nome"
              ></v-text-field>
              <v-text-field
                label="Atividade icone"
                name="atividadeIcone"
                v-model="formularioMomento.atividade.icone"
              ></v-text-field>
              <v-text-field
                label="Anotacao"
                name="anotacao"
                v-model="formularioMomento.anotacao"
              ></v-text-field>
            </v-card-text>

            <v-card-actions class="justify-content-center">
              <v-btn color="primary" depressed type="submit">Submeter</v-btn>
            </v-card-actions>
          </v-card>
        </v-form>
      </v-container>
      <v-container>
        <v-card 
          v-for="cartao in cartoesDiarios"
          :key="cartao.id"
          class="cartao-diario mb-3" 
          min-width="400"
        >
          <v-sheet :color="cartao.cor">
            <v-card-title class="cartao-diario__titulo"> {{ cartao.dataDia }} </v-card-title>
          </v-sheet>
          <v-card-text>
            <v-timeline dense>
              <v-timeline-item 
                v-for="momento in cartao.momentos"
                :key="momento.id"
                :icon="momento.humor.icone" 
                :color="momento.humor.cor"
              >
                <div>{{ momento.humor.nome }} <small>{{ momento.dataHora }}</small></div>
                <div>{{ momento.atividade.nome }}</div>
              </v-timeline-item>
            </v-timeline>
          </v-card-text>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
//import CartaoDiario from "@/components/CartaoDiario.vue";

export default {
  name: "DashboardView",
  //components: { CartaoDiario },

  data: () => ({
    cards: ["Today", "Yesterday"],
    drawer: true,
    links: [
      ["mdi-pencil", "Registros"],
      ["mdi-chart-bar", "Estatísticas"],
      ["mdi-exit-to-app", "Sair"],
    ],

    cartoesDiarios: [
      // {
      //   id: 0,
      //   dataDia: new Date(),
      //   cor: 'green',
      //   momentos: [
      //     {
      //       id: 1,
      //       humor: {
      //         icone: 'mdi-emoticon',
      //         nome: 'feliz',
      //         cor: 'green'
      //       },
      //       dataHora: new Date(),
      //       atividade: {
      //         icone: 'mdi-briefcase',
      //         nome: 'trabalho'
      //       },
      //       anotacao: 'balblablalalala'
      //     },
      //     {
      //       id: 2,
      //       humor: {
      //         icone: 'mdi-emoticon',
      //         nome: 'feliz',
      //         cor: 'green'
      //       },
      //       dataHora: new Date(),
      //       atividade: {
      //         icone: '',
      //         nome: ''
      //       },
      //       anotacao: 'blululu'
      //     },
      //   ]
      // },
      // {
      //   id: 3,
      //   dataDia: new Date(),
      //   cor: 'red',
      //   momentos: [
      //     {
      //       id: 4,
      //       humor: {
      //         icone: 'mdi-emoticon-sad',
      //         nome: 'triste',
      //         cor: 'red'
      //       },
      //       dataHora: new Date(),
      //       atividade: {
      //         icone: 'mdi-briefcase',
      //         nome: 'trabalho'
      //       },
      //       anotacao: 'balblablalalala'
      //     }
      //   ]
      // },
      // {
      //   id: 5,
      //   dataDia: new Date(),
      //   cor: 'grey',
      //   momentos: [
      //     {
      //       id: 6,
      //       humor: {
      //         icone: 'mdi-emoticon-neutral',
      //         nome: 'neutro',
      //         cor: 'grey'
      //       },
      //       dataHora: new Date(),
      //       atividade: {
      //         icone: 'mdi-briefcase',
      //         nome: 'trabalho'
      //       },
      //       anotacao: 'balblablalalala'
      //     }
      //   ]
      // }
    ],

    formularioMomento: {
      dataHora: '',
      humor: {
        nome:'',
        cor: '',
        icone: '',
      },
      atividade: {
        nome: '',
        icone: ''
      },
      anotacao: ''
    }
  }),

  methods: {
    registrarMomento() {
      console.log(this.formularioMomento);
      this.formularioMomento.dataHora = new Date();

      debugger
      const cartaoDiarioCorrespondente = this.cartoesDiarios.find(cartao => {
        return this.formularioMomento.dataHora.getFullYear() == cartao.dataDia.getFullYear()
          && this.formularioMomento.dataHora.getMonth() == cartao.dataDia.getMonth()
          && this.formularioMomento.dataHora.getDate() == cartao.dataDia.getDate()
      })

      if (cartaoDiarioCorrespondente) {
        cartaoDiarioCorrespondente.momentos.push({
          ...this.formularioMomento,
          humor: {...this.formularioMomento.humor},
          atividade: {...this.formularioMomento.atividade},
        });
      }
      else {
        const cartaoDiarioNovo = {
          dataDia: this.formularioMomento.dataHora,
          cor: 'green', // calcular valor
          momentos: [{
            ...this.formularioMomento,
            humor: {...this.formularioMomento.humor},
            atividade: {...this.formularioMomento.atividade},
          }]
        };

        this.cartoesDiarios.push(cartaoDiarioNovo);
        // talvez precise inserir no inicio do array;
      }
    }
  }
}
</script>

<style>
.bg-grey {
  background-color: #f5f5f5;
}

.cartao-diario .cartao-diario__titulo {
  font-size: 16px;
  padding: 0;
  padding-left: 25px;
}

</style>