<template>
  <DxPopup
      :visible="localPopupVisible"
      :drag-enabled="false"
      :hide-on-outside-click="true"
      :show-close-button="true"
      :width="500"
      :height="200"
      @hidden="closePopUp"
      :close-button-options="closeButtonOptions"
      title="Criar Evento"
    >
      <DxTextBox
        v-model="eventName"
        class="event_name"
        :input-attr="{ 'aria-label': 'Name' }"
        placeholder="Nome do Evento"
      />

      <!-- <div class="infos"> -->
        <!-- <div class="pessoas">
          <span>Pessoas</span>
          <DxNumberBox
            v-model="peopleCount"
            :max="30"
            :min="1"
            :show-spin-buttons="true"
            :input-attr="{ 'aria-label': 'People Count' }"
          />
        </div> -->

        <!-- <div class="dias">
          <span>Dias</span>
          <DxNumberBox
            v-model="daysCount"
            :max="30"
            :min="1"
            :show-spin-buttons="true"
            :input-attr="{ 'aria-label': 'Days Count' }"
          />
        </div> -->
      <!-- </div> -->

      <!-- <div class="infos">
        <DxTextBox
          v-model="purchaseTitle"
          class="title_compra"
          :input-attr="{ 'aria-label': 'Purchase Title' }"
          placeholder="Compra"
        /> -->

        <!-- <div class="value">
          <span>Valor em R$:</span>
          <DxNumberBox
            v-model="valueBRL"
            :max="100000"
            :min="0"
            :show-spin-buttons="true"
            :input-attr="{ 'aria-label': 'Value in BRL' }"
          />
        </div>
      </div> -->

      <DxButton
        text="Salvar Evento"
        type="success"
        styling-mode="contained"
        @click="criarEvento"
      />
    </DxPopup>
</template>

<script>
import DxTextBox from 'devextreme-vue/text-box';
import { DxPopup } from 'devextreme-vue/popup';
import DxButton from 'devextreme-vue/button';
import axios from 'axios'

export default {
  name: "CreateEvent",

  props: {
    popupVisible: Boolean
  },

  data() {
    return {
      localPopupVisible: this.popupVisible, // variável local para o controle do popup
      eventName: '',
      peopleCount: 1,
      daysCount: 1,
      purchaseTitle: '',
      valueBRL: 0
    };
  },

  watch: {
    popupVisible(newValue) {
      this.localPopupVisible = newValue;
    }
  },

  components: {
    DxPopup,
    DxTextBox,
    DxButton
  },

  methods: {

    criarEvento() {
      const novoEvento = {
        title: this.eventName,
      };

      axios.post('http://localhost:3000/cria_evento', novoEvento)
        .then(response => {
          console.log(response.data);
           this.$emit('eventoCriado');
          this.closePopUp(); 
          
        })
        .catch(error => {
          console.error('Ocorreu um erro ao criar o evento:', error);
        });
    },
    closePopUp() {
      this.localPopupVisible = false;
      this.$emit("update:popupVisible", false);
    },

  }
};
</script>

<style>

.event_name{
    margin-bottom: 10px    ;
}

</style>
