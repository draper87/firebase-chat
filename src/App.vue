<template>
  <div id="app">
    <!-- Inizio Wrapper -->
    <div class="wrapper">
      <div class="green"></div>
      <div class="grey"></div>

      <!-- Inizio WebApp -->
      <div class="col webapp">
        <div class="container">

          <!-- Inizio col left -->
          <div class="col left">

            <!-- Inizio Profilo -->
            <div class="profilo">
              <div class="avatar">
                <img src="img/avatar_1.jpg" alt="">
              </div>
              <div class="right-nav">
                <ul>
                  <li><a href="#"><em class="fas fa-circle-notch"></em></a></li>
                  <li><a href="#"><em class="fas fa-comment-alt"></em></a></li>
                  <li><a href="#"><em class="fas fa-ellipsis-v"></em></a></li>
                </ul>
              </div>
            </div>
            <!-- Fine Profilo -->

            <!-- Inizio Notifiche -->
            <div class="col notifiche">
              <div class="col bell">
                <em class="fas fa-bell-slash"></em>
              </div>
              <div class="col notifiche-text">
                <h4>Ricevi notifiche di nuovi messaggi</h4>
                <a href="#">Attiva notifiche desktop</a>
              </div>
            </div>
            <!-- Fine Notifiche -->

            <!-- Inizio finder -->
            <div class="find">
              <div class="find-wrapper">
                <label><em class="fas fa-search"></em></label>
                <input type="text" placeholder="Cerca o inizia una nuova chat">
              </div>
            </div>
            <!-- Fine finder -->


            <!-- Inizio Contatti -->
            <div class="col contatti">
              <div data-contact="0" class="col singolo-contatto">
                <div class="col avatar">
                  <img src="img/avatar_2.jpg" alt="">
                </div>
                <div class="col contatto-nome">
                  <h4>Chat 1</h4>
                  <p>Description for Chat 1</p>
                  <span>17.45</span>
                </div>
              </div>
              <div data-contact="1" class="col singolo-contatto">
                <div class="avatar">
                  <img src="img/avatar_3.jpg" alt="">
                </div>
                <div class="contatto-nome">
                  <h4>Chat 2</h4>
                  <p>Description for Chat 2</p>
                  <span>17.45</span>
                </div>
              </div>
              <div data-contact="2" class="col singolo-contatto">
                <div class="avatar">
                  <img src="img/avatar_4.jpg" alt="">
                </div>
                <div class="contatto-nome">
                  <h4>Chat 3</h4>
                  <p>Description for Chat 3</p>
                  <span>17.45</span>
                </div>
              </div>
              <div data-contact="3" class="col singolo-contatto">
                <div class="avatar">
                  <img src="img/avatar_5.jpg" alt="">
                </div>
                <div class="contatto-nome">
                  <h4>Chat 4</h4>
                  <p>Description for Chat 4</p>
                  <span>17.45</span>
                </div>
              </div>

            </div>
            <!-- Fine Contatti -->

          </div>
          <!-- Fine col left -->

          <!-- Inizio col right -->
          <div class="col right">

            <!-- Inizio header chat -->
            <div class="header-chat">
              <div class="col singolo-contatto attuale">
                <div class="col avatar ">
                  <img src="img/avatar_2.jpg" alt="">
                </div>
                <div class="col contatto-nome-header">
                  <h4>Chat 1</h4>
                  <p class="col">Ultimo accesso alle 17.30</p>
                </div>
              </div>
              <div class="col right-nav">
                <ul>
                  <li><a href="#"><em class="fas fa-search"></em></a></li>
                  <li><a href="#"><em class="fas fa-paperclip"></em></a></li>
                  <li><a href="#"><em class="fas fa-ellipsis-v"></em></a></li>
                </ul>
              </div>
            </div>
            <!-- Fine header chat -->

            <!-- Inizio Main Chat -->
            <div class="col main-chat">

              <div v-for="message in messages" v-bind:key="message.id" class="single-chat active">
                <div class="messaggio greeny" v-if="compareKey(message.key)">
                  <p class="greeny">{{ message.text }}<span class="nickname">{{ message.nickname }}</span><i class="fas fa-angle-down"></i></p>
                  <ul class="dropdown">
                    <li @click.prevent="deleteMessage(message)" href="#" class="cancella">Cancella</li>
                  </ul>
                </div>
                <div class="messaggio white" v-else>
                  <p class="white">{{ message.text }}<span class="nickname">{{ message.nickname }}</span><i class="fas fa-angle-down"></i></p>
                  <ul class="dropdown">
                    <li class="cancella">Cancella messaggio</li>
                  </ul>
                </div>
              </div>



            </div>
            <!-- Fine Main Chat -->

            <!-- Inizio write chat -->
              <div class="write-chat">
                  <div class="inviotesto">
                    <input v-model="nickname" type="text" name="" placeholder="Nickname">
                  </div>
                  <div class="inviotesto">
                    <input @keyup.13="storeMessage" v-model="messageText" type="text" name="" placeholder="Scrivi...">
                  </div>
                  <div class="send">
                    <em @click="storeMessage" class="fas fa-paper-plane"></em>
                  </div>
              </div>
            <!-- Fine write chat -->

          </div>
          <!-- Fine col right -->

        </div>
      </div>
      <!-- Fine WebApp -->

    </div>
    <!-- Fine Wrapper -->


    <!-- Template da non visualizzare -->
    <div class="tonotshow">
      <div class="messaggio">
        <p> <span></span><em class="fas fa-angle-down"></em></p>
        <ul class="dropdown">
          <li class="cancella">Cancella messaggio</li>
        </ul>
      </div>
    </div>

    <!-- Template da non visualizzare -->
  </div>
</template>

<script>
//Import the functions you need from the SDKs you need
import {initializeApp} from "firebase/app";
import { getDatabase, ref, onChildAdded, push, remove, onChildRemoved } from "firebase/database";

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "AIzaSyD7E2P-fB8XsPgbog48SGWBen01Wknm6A4",
  authDomain: "vue-school-chat-room-1e335.firebaseapp.com",
  databaseURL: "https://vue-school-chat-room-1e335-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "vue-school-chat-room-1e335",
  storageBucket: "vue-school-chat-room-1e335.appspot.com",
  messagingSenderId: "986188631810",
  appId: "1:986188631810:web:c8b6822dd8878d6e51fc1b"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const messages = ref(db, 'messages');

export default {
  name: 'App',
  props: {
    msg: String
  },
  data() {
    return {
      messages: [],
      messageText: '',
      nickname: '',
      key: localStorage.getItem('myPairKey')
    }
  },
  methods: {
    storeMessage () {
      push(messages, {text: this.messageText, nickname: this.nickname, key: this.key})
      this.messageText = ''
    },
    deleteMessage (message) {
      let messageToBeDeleted = ref(db, 'messages/' + message.id);
      remove(messageToBeDeleted);
    },
    compareKey(key) {
      return key === this.key
    },
  },
  created () {
    onChildAdded(messages, snapshot => this.messages.push({...snapshot.val(), id: snapshot.key}));
    onChildRemoved(messages, snapshot => {
      const deletedMessage = this.messages.find(message => message.id === snapshot.key)
      const index = this.messages.indexOf(deletedMessage);
      this.messages.splice(index, 1);
    })
  },
  beforeCreate () {
    function randomPairKey(length) {
      let result = '';
      const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      const charactersLength = characters.length;
      for (let i = 0; i < length; i++ ) {
        result += characters.charAt(Math.floor(Math.random() *
            charactersLength));
      }
      return result;
    }

    let key = randomPairKey(5);

    if (!localStorage.getItem('myPairKey')) {
      localStorage.setItem('myPairKey', key)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
@media only screen and (max-device-width: 768px) {
  .col.left {
    display: none;
  }
  .col.right {
    width: 100%;
  }
  .white,.greeny {
    font-size: 2rem;
  }
  .nickname {
    font-size: 1rem !important
  }
  .write-chat .inviotesto {
    margin-left: 5px;
    flex-basis: 40%;
  }
  .send {
    padding-right: 30px;
  }
}
</style>
