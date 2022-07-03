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
        <v-row class="justify-content-center" no-gutters>
          <v-card width="600px">
            <template v-if="deveExibirOsCampos">
              <v-form @submit.prevent="registrarMomento">
                <v-card-text>
                  <label class="font-weight-bold">Selecione um humor:</label>
                  <v-btn-toggle 
                    v-model="formularioMomento.humor"
                    class="d-flex justify-content-space-around"
                  > 
                    <div 
                      v-for="humor in humores"
                      :key="humor.id"
                      class="d-flex flex-direction-column align-items-center"
                    >
                      <v-btn
                        icon
                        :color="humor.cor"
                        :value="humor"
                        x-large
                      >
                        <v-icon 
                          :color="humor.cor"
                          x-large
                        >
                        {{ humor.icone }}
                        </v-icon>
                      </v-btn>
                      <div :class="'text-uppercase font-weight-bold ' + humor.cor + '--text'">
                        {{ humor.nome }}
                      </div>
                    </div>
                  </v-btn-toggle>

                  <div class="mt-5">
                    <label class="font-weight-bold">Selecione uma atividade:</label>
                    <v-chip-group
                      active-class="primary--text"
                      column
                      v-model="formularioMomento.atividade"
                    >
                      <v-chip 
                        v-for="atividade in atividades"
                        :key="atividade.id"
                        label
                        :value="atividade"
                      >
                        <v-icon left>
                          {{ atividade.icone }}
                        </v-icon>
                        {{ atividade.nome }}
                      </v-chip>
                    </v-chip-group>
                  </div>
                  <div class="mt-5">
                    <v-text-field
                      label="Nota"
                      name="anotacao"
                      v-model="formularioMomento.anotacao"
                      outlined
                      dense
                    ></v-text-field>  
                  </div>
                </v-card-text>

                <v-card-actions class="justify-content-end">
                  <v-btn @click="cancelarFormulario" color="error" text>Cancelar</v-btn>
                  <v-btn color="primary" type="submit" text>Adicionar</v-btn>
                </v-card-actions>
              </v-form>
            </template>
            <template v-else>
              <v-card-text>
                <v-btn
                  @click="deveExibirOsCampos = true"
                  text
                >
                  <v-icon
                    left
                  >
                  mdi-plus
                  </v-icon>
                  Adicionar um momento
                </v-btn>
              </v-card-text>
            </template>
          </v-card>
        </v-row>
        <v-row class="justify-content-center" no-gutters>
          <v-card width="600px" 
            v-for="cartao in cartoesDiarios"
            :key="cartao.id"
            class="cartao-diario mt-3" 
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
                  <div>{{ momento.anotacao }}</div>
                </v-timeline-item>
              </v-timeline>
            </v-card-text>
          </v-card>
        </v-row>
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
    },

    atividades: [
      {id: 0, nome: "Trabalho", icone: "mdi-briefcase"},
      {id: 1, nome: "Casa", icone: "mdi-home"},
      {id: 2, nome: "Universidade", icone: "mdi-school"},
      {id: 3, nome: "Atividade física", icone: "mdi-weight-lifter"},
    ],

    humores: [
      {id: 0, nome: "Muito feliz", cor: "green", icone: "mdi-emoticon-outline" },
      {id: 1, nome: "Neutro", cor: "grey", icone: "mdi-emoticon-neutral-outline" },
      {id: 2, nome: "Triste", cor: "red", icone: "mdi-emoticon-sad-outline" },
    ],

    deveExibirOsCampos: false,
  }),

  methods: {
    registrarMomento() {
      console.log(this.formularioMomento);
      this.formularioMomento.dataHora = new Date();

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

      this.resetarMomento();
      this.deveExibirOsCampos = false;
    },

    resetarMomento() {
      this.formularioMomento.dataHora = '';
      this.formularioMomento.humor = { nome: '', cor: '', icone: '' };
      this.formularioMomento.atividade = { nome: '', icone: '' };
      this.formularioMomento.anotacao = ''; 
    },

    cancelarFormulario() {
      this.resetarMomento();
      this.deveExibirOsCampos = false;
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