<template>
  <div class="app">
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
            :text="event.title"
            type="normal"
            styling-mode="contained"
            class="button_title"
          />

          <DxButton
            :width="50"
            text="X"
            type="danger"
            styling-mode="contained"
            @click="confirmDelete(event.id)" 
          />
        </div>
      </div>
    </div>

    <!-- Popup de confirmação para deletar o evento -->
    <DxPopup
      :visible="confirmDeletePopupVisible"
      :drag-enabled="false"
      :hide-on-outside-click="false"
      :show-close-button="true"
      :width="400"
      :height="200"
      title="Confirmar Exclusão"
    >
      <div class="popup-content">
        <p>Tem certeza que deseja excluir este evento?</p>
        <DxButton
          text="Sim, excluir"
          type="danger"
          styling-mode="contained"
          @click="deleteEvent"  
        />
        <DxButton
          text="Cancelar"
          type="normal"
          styling-mode="contained"
          @click="closeDeletePopup"
        />
      </div>
    </DxPopup>

    <CreateEvent v-model:popupVisible="popupVisible" @eventoCriado="fetchEvents" />

  </div>
</template>

<script>
import DxButton from 'devextreme-vue/button';
import CreateEvent from "../components/create-event.vue";
import { DxPopup } from 'devextreme-vue/popup';
import axios from 'axios';

export default {
  name: "EventsPage",

  components: {
    DxButton,
    CreateEvent,
    DxPopup
  },

  data() {
    return {
      events: [],
      popupVisible: false,
      confirmDeletePopupVisible: false,  // Controle da visibilidade do popup de confirmação
      eventIdToDelete: null  // Armazena o ID do evento a ser deletado
    };
  },

  methods: {
    openPopUp() {
      this.popupVisible = true;
    },

    // Método para abrir o popup de confirmação
    confirmDelete(id) {
      this.eventIdToDelete = id;  // Armazena o ID do evento que será deletado
      this.confirmDeletePopupVisible = true;  // Exibe o popup
    },

    // Método para realmente deletar o evento
    deleteEvent() {
      axios.delete(`http://localhost:3000/deleta_evento/${this.eventIdToDelete}`)
        .then(response => {
          console.log(response.data);
          this.fetchEvents();  // Atualiza a lista de eventos
          this.closeDeletePopup();  // Fecha o popup de confirmação
        })
        .catch(error => {
          console.error('Ocorreu um erro ao deletar o evento:', error);
        });
    },

    // Fecha o popup de exclusão
    closeDeletePopup() {
      this.confirmDeletePopupVisible = false;
      this.eventIdToDelete = null;  // Reseta o ID
    },

    fetchEvents() {
      axios.get("http://localhost:3000/consulta_evento")
        .then(response => {
          this.events = response.data;
        })
        .catch(err => {
          console.log(err);
        });
    },

    closePopUp() {
      this.popupVisible = false;
      this.fetchEvents()
    }
  },

  mounted() {
    this.fetchEvents(); // Carrega os eventos quando o componente é montado
  }
};
</script>

<style lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.dx-card {
  background-color: #535353;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.buttons-events {
  margin-bottom: 10px;
}

.app {
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.button_title {
  margin-right: 5px;
}
</style>
