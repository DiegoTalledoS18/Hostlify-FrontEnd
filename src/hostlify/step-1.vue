<template>
  <div class="containerA">
    <h1 style="font-weight: bolder;color: #D6A049">{{$t("request service")}}</h1>
    <div class="row" style="">
      <div class="">
        <div class="wrapper-progressBar">
          <ul class="progressBar">
            <li class="active">{{$t("select your dish")}}</li>
            <li >{{$t("select your drink")}}</li>
            <li >{{$t("select your cream")}}</li>
            <li >{{$t("confirmation")}}</li>
          </ul>
        </div>
      </div>
    </div>
    <div style="display: flex; justify-content: space-around;margin-top: 3rem;" >
      <pv-card style="width: 50vw; border-radius: 1rem">
        <template #content>
          <div style="display: flex; justify-content:left">
            <h2>{{$t("room")}}</h2>
          </div>
          <div style="display: flex; justify-content:center">
            <h1 style="text-align: center">{{ roomName }}</h1>
          </div>
          <div style="display: flex; justify-content:left">
            <h2>{{$t("select your dish")}}</h2>
          </div>
          <div style="display: flex;margin-bottom:30px; justify-content:center">
            <pv-dropdown @change="actualizarEstadoBoton" v-model="selectedDish" :options="dishes"  placeholder="Eliga un platillo" style="width:500px"></pv-dropdown>
          </div>
          <div style="display: flex; justify-content:center">
            <br>
            <pv-input-number v-model="dishQuantity" showButtons buttonLayout="horizontal" min="1"
                             incrementButtonIcon="pi pi-plus" decrementButtonIcon="pi pi-minus" mode="decimal" />
          </div>
          <div style="display: flex; justify-content:center">
            <pv-button :disabled="!esFormularioCompleto" class="button" style="border-radius: 0.4rem; color:white;font-weight:bold;margin-top: 1.5rem" @click="next">{{$t("next")}}</pv-button>
          </div>
          <div style="display: flex; justify-content:center">
            <router-link style="text-decoration:none; color:#D6A049" to="/services"> <h6>{{$t("cancel")}}</h6> </router-link>
          </div>
        </template>
      </pv-card>

    </div>
  </div>

</template>


<script>
import {RoomServices} from "../services/room-services";
export default {
  name: "step-1",
  data(){
    return{
        token: sessionStorage.getItem("jwt"),
      roomName:null,
      dishes:["Ceviche","Lomo saltado","Aji de gallina","Causa Limeña","Arroz con pollo","Tallarines verdes","Pollo a la brasa"],
      selectedDish:null,
      dishQuantity:1,
      esFormularioCompleto: false
    }
  },
  created() {
    new RoomServices().getRoomForGuest(this.token, sessionStorage.getItem("id")).then(response=> {
      this.roomName=response.data.roomName
    })

  },
  methods:{
    next(){
      sessionStorage.setItem("dish",this.selectedDish)
      sessionStorage.setItem("dishQuantity",this.dishQuantity)
      this.$router.push("/step-2")
    },
    actualizarEstadoBoton() {
      this.esFormularioCompleto = (this.selectedDish!=null);
    },
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap');

.wrapper-progressBar {
  width: 97vw;
  height: 10vh;
}

.progressBar {
}

.progressBar li {
  list-style-type: none;
  float: left;
  width: 22%;
  position: relative;
  text-align: center;
}

.progressBar li:before {
  content: " ";
  line-height: 30px;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  border: 1px solid #ddd;
  display: block;
  text-align: center;
  margin: 0 auto 10px;
  background-color: white
}

.progressBar li:after {
  content: "";
  position: absolute;
  width: 100%;
  height: 4px;
  background-color: #ddd;
  top: 15px;
  left: -50%;
  z-index: -1;
}

.progressBar li:first-child:after {
  content: none;
}

.progressBar li.active {
  color: #D6A049;
}

.progressBar li.active:before {
  border-color: #D6A049;
  background-color: #D6A049
}

.progressBar .active:after {
  background-color: #D6A049;
}

.containerA{
  font-family: 'Roboto', sans-serif;
  font-weight: bold;
  padding: 3rem;
}

</style>