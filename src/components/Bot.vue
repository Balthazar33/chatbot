<template>
  <div id="app_wrapper">
    <!-- MAIN APP WRAPPER -->
    <transition name="fade">
    <div class="chatbot_container" v-if="chat_bot_open">
      <div class="bg_top_orange">
        <div class="intro_txt_wrapper">
          <p class="bot_title txt-bg">IRIS</p>
          <p class="txt-md hello_txt text-bold">Hello</p>
          <img src="@/assets/images/hand_wave.png" alt />
          <p class="iris_intro">
            I am Iris, a Virtual Assistant
            <br />How may I help you?
          </p>
        </div>
      </div>

      <!-- BOT ICON -->
      <div class="central_robot ellipse">
        <img src="@/assets/images/Vector.svg" alt />
      </div>
      <div class="central_robot_hidden ellipse">
        <img src="@/assets/images/Vector.svg" alt />
      </div>

      <!-- FRONT CARD -->

      <div class="front_bottom_card" id="front_card">

        <div id="faqs" v-if="hide_input==true">
          <div class="faqs_title text-bold">
            <p>
              Frequently asked questions
              <img
                src="@/assets/images/question_mark.svg"
                alt="Question mark"
              />
            </p>
          </div>

          <!-- FAQ items -->
          <div class="faq_items">
            <div
              class="faq_item"
              v-for="(item,index) in questions"
              :key="'item'+index"
              @click="faq_clicked(index)"
            >
              <span class="bullet_mark"></span>
              <span>{{item}}</span>
            </div>
          </div>
        </div>

        <!-- CHATS SECTION -->
        <div
          id="chats_wrapper"
          :class="hide_input===false?'card_scroll':''"
          v-if="hide_input===false"
        >
          <div class="chat_Container">
            <div class="chat clearfix" v-for="(entry,index) in chats" :key="'entry'+index">
              <!-- Check if user or bot, place accordingly -->
              <span :class="entry.user_or_bot=='bot'? 'bot_bubble':'user_bubble'">{{entry.text}}</span>
            </div>
          </div>
        </div>

        <!-- INPUT SECTION -->

        <div class="input_box">
          <div class="start_conv" @click="hide_input=false" v-if="hide_input==true">
            <img src="@/assets/images/chat_icon.svg" alt="Chat icon" />
            <p>Start a new conversation</p>
          </div>

          <!-- INPUT FIELD -->
          <div id="input_bottom" v-if="!hide_input">
            <input type="text" name="chat_input" v-model="user_input" @keyup.enter="send_Clicked" />
            <a href="javascript:void(0);" @click="send_Clicked" class="send_btn">
              <i class="fa fa-paper-plane"></i>
            </a>

            <!-- GO BACK BUTTON -->
            <a href="javascript:void(0);" class="back_link" @click="reset_chat">
              <i class="fa fa-backward"></i>
            </a>
          </div>
        </div>
      </div>
    </div>
    </transition>
    <div class="open_chatbot ellipse" @click="open_chatbot">
        <img src="@/assets/images/Vector.svg" alt />
      </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Bot",
  props: {
    msg: String,
  },
  data() {
    return {
      questions: [
        "Can I redeem my FB before the original term?",
        "How do I pay my Credit card bill?",
        "How can I get my Account Statement?",
        "What is the tenure of Fixed Deposit?",
      ],
      user_input: "",
      hide_input: true,
      data: [],
      message:"Bot message",
      botReply: [],
      user_entry: [],
      nomatch: 0,
      chats: [],
      match_found: "",
      chat_bot_open:false,
    };
  },
  methods: {
    open_chatbot(){
      this.chat_bot_open = !this.chat_bot_open;
    },
    send_Clicked(index) {
      this.match_found = "";

      var self = this;
      if (self.user_input != "") {
        self.chats.push({
          text: self.user_input,
          user_or_bot: "user",
        });

        axios
          .get(
            "https://my-json-server.typicode.com/Balthazar33/myjson_json/inputs"
          )
          .then((response) => {
            this.data = response.data;
            for (var i = 0; i < self.data.length; i++) {
              //loop over received response items
              for (var j = 0; j < self.data[i]["user_input"].length; j++) {
                //loop over user_input array for each
                if (self.data[i].user_input[j] == self.user_input.toLowerCase()) {
                  //Match is found

                  if (self.data[i].tag == "finance") {
                    //If it is a question of finance category...
                    self.chats.push({
                      text: self.data[i].responses[index], //...provide accurate response
                      user_or_bot: "bot",
                    });
                    self.match_found = self.data[i].responses[index];
                  } else {
                    var randomint = Math.floor(
                      Math.random() * self.data[i].responses.length
                    ); //random integer from 0 to length of response array
                    self.chats.push({
                      text: self.data[i].responses[randomint],
                      user_or_bot: "bot",
                    });
                    self.match_found = self.data[i].responses[randomint];
                  }

                  self.nomatch = 0; //Update match flag
                } else {
                  //If match is not found, set matchfound flag
                  self.match_found == ""
                    ? (self.nomatch = 1)
                    : (self.nomatch = 0);
                }
              }
            }
            self.user_input = ""; //Clear input field

            if (self.nomatch === 1) {
              //If no match is found, print default
              self.chats.push({
                text: "Sorry, didn't get that.",
                user_or_bot: "bot",
              });
            }

            //Make div scroll to top
            var front_card = document.getElementById("chats_wrapper");
            front_card.scrollTop = front_card.scrollHeight + 200;
          });
      }
    },
    faq_clicked(index) {
      this.user_input = this.questions[index]; //Use preset faq question for chat
      this.hide_input = false; //Make faqs invisible
      this.send_Clicked(index);
    },
    reset_chat() {
      this.hide_input = true; //Go back to start page
      this.chats = [];
    },
  },
};
</script>

<style scoped>
@import "../assets/style.css";

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity:0;
}
</style>