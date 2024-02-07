<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title id="titol">COMPTADOR</ion-title>
      <ion-buttons slot="primary">
      <ion-button color="primary" fill="solid" @click="info">
        <ion-icon :icon="infoIcon"></ion-icon>
      </ion-button>
      </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="custom-font">
      <ion-header class="ion-no-border ion-padding-top ion-padding-horizontal">
        <ion-grid>
          <ion-row>
            <ion-col>
              <div class="ion-text-start">
                Punts: {{score}}
              </div>
            </ion-col>
            <ion-col>
              <div class="ion-text-end">
                Temps restant: {{timeleft}}
              </div>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-header>

      <div id="container">
        <strong>Clica per a sumar punts!</strong> <br>
        <br>
        <ion-button id="tapMeButton" color="primary" @click="tap" class="custom-font">
          Clica'm
        </ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  alertController,
  IonButton,
  IonCol,
  IonGrid,
  IonButtons,
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  toastController
} from '@ionic/vue';
import { defineComponent } from "vue";
import { informationCircleOutline } from 'ionicons/icons';
import { createAnimation } from '@ionic/vue';
const INITIAL_TIME = 5

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonButtons,
    IonButton,
    IonGrid,
    IonTitle,
    IonCol,
    IonToolbar
  },
  setup() {
    return {
      infoIcon: informationCircleOutline,
      started: false,
      counterInterval: null
    }
  },
  data() {
    return {
      score: 0,
      timeleft: INITIAL_TIME
    }
  },
  methods: {
    bounce () {
      const animation = createAnimation()
      animation.addElement(document.getElementById('tapMeButton'))
        .duration(1000)
        .fromTo('transform', 'scale(0.9)', 'scale(1.0)')
      animation.play()
    },
    async info() {
      const alert = await alertController.create({
        header: 'Informació',
        subHeader: 'App creada per jojo',
        message: 'Aplicació per a fer clicks',
        buttons: ['OK']
      });
      await alert.present();
    },
    tap() {
      this.bounce()
      this.score++;
      if (!this.started) {
        this.counterInterval = setInterval(() => {
          this.timeleft--
          console.log('A')
          console.log(this.timeleft);
        }, 1000)
        this.started = true
      }
    },
    async showResult() {
      const toast = await toastController.create({
        color: 'dark',
        duration: 2000,
        message: `Temps acabat. Has fet ${this.score} clicks`,
        showCloseButton: true
      });
      await toast.present();
    },
  },
  watch: {
    timeleft: function (newTimeLeft) {
      if (newTimeLeft <= 0) {
        console.log('FINAL')
        this.started = false
        this.timeleft = INITIAL_TIME
        clearInterval(this.counterInterval)
        this.showResult()
        this.score = 0
      }
    }
  }
});
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css?family=Karla:400');

.custom-font {
  font-family: 'Karla', sans-serif;
}

#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#titol {
  letter-spacing: 2px;
  font-family: 'Karla', sans-serif;
  margin-left: 3px;
  color: #7974ff;
  font-weight: bold;
}

#container a {
  text-decoration: none;
}
</style>
