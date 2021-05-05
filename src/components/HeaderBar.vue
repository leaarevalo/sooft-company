<template>
  <v-container class="container" fluid>
    <v-form v-model="validForm">
      <v-row>
        <p class="title">Reservar</p>
      </v-row>
      <v-row>
        <v-col cols="12" sm="4">
          <v-text-field
            outlined
            v-model="name"
            :height="36"
            label="Nombre"
            :rules="required"
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="2">
          <v-menu
            ref="menuDate"
            v-model="dateMenu"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="date"
                label="Picker in menu"
                append-icon="mdi-calendar"
                readonly
                :rules="required"
                outlined
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="date"
              color="green lighten-1"
            ></v-date-picker>
          </v-menu>
        </v-col>
        <v-col cols="12" sm="2">
          <v-menu
            ref="menuSince"
            v-model="menuSince"
            :close-on-content-click="false"
            :nudge-right="40"
            :return-value.sync="hourStart"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="hourStart"
                label="Desde"
                append-icon="mdi-clock-outline"
                readonly
                :rules="required"
                outlined
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-time-picker
              v-if="menuSince"
              v-model="hourStart"
              format="24hr"
              color="green lighten-1"
              full-width
              @click:minute="$refs.menuSince.save(hourStart)"
            ></v-time-picker>
          </v-menu>
        </v-col>
        <v-col cols="12" sm="2">
          <v-menu
            ref="menuFinish"
            v-model="menuFinish"
            :close-on-content-click="false"
            :nudge-right="40"
            :return-value.sync="hourFinish"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="hourFinish"
                label="Hasta"
                append-icon="mdi-clock-outline"
                readonly
                :rules="required"
                outlined
                heigth="36px"
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-time-picker
              v-if="menuFinish"
              v-model="hourFinish"
              color="green lighten-1"
              format="24hr"
              full-width
              @click:minute="$refs.menuFinish.save(hourFinish)"
            ></v-time-picker>
          </v-menu>
        </v-col>
        <v-col cols="12" sm="2">
          <v-btn medium color="success" :disabled="!validForm" @click="saveData"
            >Guardar</v-btn
          >
        </v-col>
      </v-row>
    </v-form>
    <v-alert
      color="red"
      type="error"
      border="bottom"
      colored-border
      elevation="10"
      v-model="showAlert"
      >El horario no es correcto</v-alert
    >
  </v-container>
</template>

<script>
import moment from "moment";
export default {
  name: "HeaderBar",
  data() {
    return {
      loading: false,
      showAlert: false,
      validForm: false,
      name: "",
      date: null,
      dateMenu: false,
      menuSince: false,
      menuFinish: false,
      hourStart: null,
      hourFinish: null,
      id: 0,
      required: [(v) => !!v || "Requerido"],
    };
  },
  methods: {
    saveData() {
      if (this.checkData()) {
        const dataToSend = {
          id: this.id,
          name: this.name,
          date: this.date,
          hourStart: this.hourStart,
          hourFinish: this.hourFinish,
          toDelete: false,
        };
        this.id++;
        this.reset();
        this.$emit("saveData", dataToSend);
      }
    },
    checkData() {
      var startTime = moment(this.hourStart, "hh:mm");
      var endTime = moment(this.hourFinish, "hh:mm");
      let result = startTime.isAfter(endTime);
      console.log(result);
      if (result) {
        this.showAlert = true;
      } else {
        return true;
      }
      setTimeout(() => {
        this.showAlert = false;
      }, 4000);
    },
    reset() {
      this.name = "";
      this.hourStart = null;
      this.hourFinish = null;
    },
  },
};
</script>

<style lang="sass" scoped>
@import url('https://fonts.googleapis.com/css2?family=Anton&display=swap')
.container
  width: 1200px
  margin: 0 auto
  margin-top: 30px
  height: 200px
  border: 1px solid rgb(180, 180, 180)
  border-radius: 8px
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.2)
  margin-bottom: 30px
.title
  font-family: 'Roboto', sans-serif
  margin-left: 14px
  margin-top: 20px
  color: green
</style>