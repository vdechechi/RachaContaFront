<template>
  <div>
    <div class="container">
      <div class="dx-card header responsive-paddings">
        <DxButton
          :width="240"
          text="Criar Evento"
          type="success"
          styling-mode="contained"
          @click="openPopUp"
        />
      </div>
      <div class="dx-card events responsive-paddings">
        <div class="buttons-events" v-for="(event, index) in events" :key="index">
          <DxButton
          :width="240"
          :text="event"
          type="normal"
          styling-mode="contained"
        />
        </div>
      </div>
    </div>
  </div>
<DxPopup
  :visible="popupVisible"
  :drag-enabled="false"
  :hide-on-outside-click="false"
  :show-close-button="false"
  :width="300"
  :height="500"
  @hidden="closePopUp"
>

<DxToolbarItem
  widget="dxButton"
  toolbar="bottom"
  location="after"
  :options="{
    text: 'Fechar',
    stylingMode: 'contained',
    type: 'danger',
    onClick: () => {
      this.popupVisible = false;
    }
  }"
/>
 

       <DxToolbarItem
  widget="dxButton"
  toolbar="bottom"
  location="before"
  :options="{
    text: 'Criar Evento',
    stylingMode: 'contained',
    type: 'success',
    onClick: () => {
      this.popupVisible = false;
    }
  }"
/>


</DxPopup>


  
</template>

<script>
import DxButton from 'devextreme-vue/button';
import { DxPopup, DxToolbarItem} from 'devextreme-vue/popup';
// import axios from axios;

export default {
  name: "EventsPage", // Nome do componente

  components: {
    DxButton,
    DxPopup,
    DxToolbarItem
  },

  data() {
    return {
      events: ["Balada", "Praia", "Carnaval", "Comida"],
      popupVisible: false,
      closeButtonOptions:{
          
},

    };
  },

 methods: {
  openPopUp() {
    this.popupVisible = true;
  },

  closePopUp() {
    this.popupVisible = false;
  }
},


  mounted() {
    // Este hook é chamado quando o componente é montado
    console.log("Componente montado!");
  }
};
</script>

<style lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center; /* Centraliza o conteúdo horizontalmente */
  justify-content: center; /* Centraliza o conteúdo verticalmente */
  height: 100vh; /* Garante que a centralização funcione em toda a altura da tela */
}

.dx-card {
  background-color: #535353;
  display: flex;
  flex-direction: column;
  align-items: center; /* Centraliza os botões dentro do card */
  margin-bottom: 20px; /* Espaçamento entre os cards */
}

.buttons-events {
  margin-bottom: 10px;
}
</style>
