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
                  <v-row>
                    <v-col>
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
                    </v-col>
                  </v-row>

                  <v-row>
                    <v-col>
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
                    </v-col>
                  </v-row>

                  <v-row>
                    <v-col cols="6">
                      <v-menu
                        ref="menu"
                        v-model="formularioMomento.menuDataDia"
                        :close-on-content-click="false"
                        :return-value.sync="formularioMomento.dataDia"
                        transition="scale-transition"
                        offset-y
                        min-width="auto"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-text-field
                            v-model="formularioMomento.dataDia"
                            label="Selecione uma data"
                            prepend-icon="mdi-calendar"
                            readonly
                            v-bind="attrs"
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-date-picker
                          v-model="formularioMomento.dataDia"
                          no-title
                          scrollable
                        >
                          <v-spacer></v-spacer>
                          <v-btn
                            text
                            color="primary"
                            @click="formularioMomento.menuDataDia = false"
                          >
                            Cancel
                          </v-btn>
                          <v-btn
                            text
                            color="primary"
                            @click="$refs.menu.save(formularioMomento.dataDia)"
                          >
                            OK
                          </v-btn>
                        </v-date-picker>
                      </v-menu>
                    </v-col>
                    <v-col cols="6">
                      <v-menu
                        ref="menuHora"
                        v-model="formularioMomento.menuDataHora"
                        :close-on-content-click="false"
                        :nudge-right="40"
                        :return-value.sync="formularioMomento.dataHora"
                        transition="scale-transition"
                        offset-y
                        max-width="290px"
                        min-width="290px"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-text-field
                            v-model="formularioMomento.dataHora"
                            label="Picker in menu"
                            prepend-icon="mdi-clock-time-four-outline"
                            readonly
                            v-bind="attrs"
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-time-picker
                          v-if="formularioMomento.menuDataHora"
                          v-model="formularioMomento.dataHora"
                          full-width
                          format="24hr"
                          @click:minute="$refs.menuHora.save(formularioMomento.dataHora)"
                        ></v-time-picker>
                      </v-menu>
                    </v-col>
                  </v-row>

                  <v-row>
                    <v-col>
                      <v-text-field
                        label="Nota"
                        name="anotacao"
                        v-model="formularioMomento.anotacao"
                        outlined
                        dense
                      ></v-text-field>  
                    </v-col>
                  </v-row>
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
        <v-row class="flex-direction-column align-items-center" no-gutters>
          <v-card width="600px" 
            v-for="(cartao, cartaoIndex) in cartoesDiarios"
            :key="cartao.id"
            class="cartao-diario mt-3" 
            min-width="400"
          >
            <v-sheet :color="cartao.cor">
              <v-card-title class="cartao-diario__titulo"> {{ cartao.dataDia | date }} </v-card-title>
            </v-sheet>
            <v-card-text>
              <v-timeline dense>
                <v-timeline-item 
                  v-for="(momento, momentoIndex) in cartao.momentos"
                  :key="momento.id"
                  :icon="momento.humor.icone" 
                  :color="momento.humor.cor"
                >
                  <div class="d-flex justify-space-between">
                    <div>{{ momento.humor.nome }}<small> — {{ momento.data | fullDate }}</small></div>
                    <v-menu
                      bottom
                      right
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          x-small
                          icon
                          v-bind="attrs"
                          v-on="on"
                        >
                          <v-icon>mdi-dots-vertical</v-icon>
                        </v-btn>
                      </template>

                      <v-list>
                        <v-list-item>
                          <v-list-item-title @click="editarMomento">Editar</v-list-item-title>
                        </v-list-item>
                        <v-list-item @click="excluirMomento(momento, momentoIndex, cartao, cartaoIndex)">
                          <v-list-item-title>Excluir</v-list-item-title>
                        </v-list-item>
                      </v-list>
                    </v-menu>
                  </div>
                  <div class="my-1">
                    <v-chip
                      small
                    >
                      <v-avatar left>
                        <v-icon small> {{ momento.atividade.icone }} </v-icon>
                      </v-avatar>
                      {{ momento.atividade.nome }}
                    </v-chip>
                  </div>
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
import moment from 'moment'; 

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

    cartoesDiarios: [],

    formularioMomento: {
      dataDia: '',
      menuDataDia: false,

      dataHora: '',
      menuDataHora: false,

      data: '',

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
    time: null,
  }),

  methods: {
    registrarMomento() {
      this.formularioMomento.data = new Date(this.formularioMomento.dataDia + 'T' + this.formularioMomento.dataHora);

      const cartaoDiarioCorrespondente = this.cartoesDiarios.find(cartao => {
        return this.formularioMomento.data.getFullYear() == cartao.dataDia.getFullYear()
          && this.formularioMomento.data.getMonth() == cartao.dataDia.getMonth()
          && this.formularioMomento.data.getDate() == cartao.dataDia.getDate()
      })

      if (cartaoDiarioCorrespondente) {
        cartaoDiarioCorrespondente.momentos.push({
          data: this.formularioMomento.data,
          humor: {...this.formularioMomento.humor},
          atividade: {...this.formularioMomento.atividade},
          anotacao: this.formularioMomento.anotacao,
        });
      }
      else {
        const cartaoDiarioNovo = {
          id: this.cartoesDiarios.length,
          dataDia: this.formularioMomento.data,
          cor: 'green', // calcular valor
          momentos: [{
            data: this.formularioMomento.data,
            humor: {...this.formularioMomento.humor},
            atividade: {...this.formularioMomento.atividade},
            anotacao: this.formularioMomento.anotacao,
          }]
        };

        this.cartoesDiarios.push(cartaoDiarioNovo);
        // talvez precise inserir no inicio do array;
      }

      this.resetarMomento();
      this.deveExibirOsCampos = false;
    },

    resetarMomento() {
      this.formularioMomento.dataDia = '';
      this.formularioMomento.menuDataDia = false;
      this.formularioMomento.dataHora = '';
      this.formularioMomento.menuDataHora = false;
      this.formularioMomento.data = null;
      this.formularioMomento.humor = { nome: '', cor: '', icone: '' };
      this.formularioMomento.atividade = { nome: '', icone: '' };
      this.formularioMomento.anotacao = ''; 
    },

    cancelarFormulario() {
      this.resetarMomento();
      this.deveExibirOsCampos = false;
    },

    excluirMomento(momento, momentoIndex, cartaoDiario, cartaoIndex) {
      cartaoDiario.momentos.splice(momentoIndex, 1);

      if (cartaoDiario.momentos.length == 0) {
        this.cartoesDiarios.splice(cartaoIndex, 1);
      }
    },

    editarMomento() {

    }

  },
  filters: {
    fullDate: function (date) {
      moment.locale('pt-br');
      return moment(date).format('LLLL');
  },
    date: function (date) {
      moment.locale('pt-br');
      return moment(date).format('L');
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