<template>
  <div id="app_wrapper">
    <!-- MAIN APP WRAPPER -->
    <transition name="fade">
    <div class="chatbot_container" v-if="chat_bot_open">
      <div>
        <div class="intro_txt_wrapper">
          
            <header>
              <h1>HELLO</h1>
              <p>Welcome to Vue-Bot</p>
            </header>
          
        </div>
      </div>


      <!-- FRONT CARD -->

      <div class="front_bottom_card" id="front_card">
        <transition name="fade">
        <div id="faqs" v-if="hide_input==true">
          <div class="faqs_title text-center text-bold">
            <p>
              FAQs
            </p>
          </div>

          <!-- FAQ items -->
         <faq-items  @faq-clicked="faq_clicked" :questions="questions"></faq-items>
        </div>
        </transition>

        <!-- CHATS SECTION -->
        <chat-section :isTyping="isTyping" :chats="chats" :hide_input="hide_input"></chat-section>
        <!-- INPUT SECTION -->

        <div class="input_box" :class="!hide_input?'to_bottom':''">
          <div class="start_conv" @click="hide_input=false" v-if="hide_input==true">
          
            <p> <i class="fa fa-commenting"></i> Begin chat</p>
          </div>

          <!-- INPUT FIELD -->
          <div id="input_bottom" v-if="!hide_input">
            <input type="text" name="chat_input" v-model.trim="user_input" @keyup.enter="send_Clicked" />
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

    <!-- OPEN CHATBOT -->
    <open-chatbot @open-chatbot="open_chatbot"></open-chatbot>
  </div>
</template>

<script>
import axios from "axios";
import OpenChatbot from "@/components/OpenChatbot.vue";
import ChatSection from "@/components/ChatSection.vue";
import FaqItems from "@/components/FaqItems.vue";
export default {
  name: "Bot",
  props: {
    msg: String,
  },
  components:{
    OpenChatbot,ChatSection,FaqItems
  },
  data() {
    return {
      questions: [
],    isTyping:false,
      user_input: "",
      hide_input: true,
      responseData: [],
      message:"Bot message",
      botReply: [],
      user_entry: [],
      nomatch: 0,
      chats: [],
      match_found: "",
      chat_bot_open:false,
    };
  },
  mounted(){
    this.getQuestions();
  },
  methods: {
    getQuestions(){
      const self = this;
      axios.get("https://my-json-server.typicode.com/Balthazar33/myjson_json/inputs?tag=vue_faqs")
      .then(response=>{
        console.log(response);
        self.questions = response.data[0].user_input;
      });
    },
    open_chatbot(){
      this.chat_bot_open = !this.chat_bot_open;
    },
    send_Clicked(index,faqClicked) {
      this.match_found = "";
      const lowerCasedInput = this.user_input.toLowerCase();
      if(!lowerCasedInput.endsWith('?') && (lowerCasedInput.startsWith('wh') || lowerCasedInput.startsWith('can') || lowerCasedInput.startsWith('how'))){
        this.user_input = this.user_input+'?';
      }
      var self = this;
      if (self.user_input != "") {
        self.chats.push({
          text: self.user_input,
          user_or_bot: "user",
        });
        this.isTyping = true;
        const isPreset = this.questions.includes(this.user_input);
        isPreset? index=this.questions.indexOf(this.user_input):'';
        if(!faqClicked && !isPreset){ //IF INPUT IS NOT PRESET
          axios
          .get(
            "https://my-json-server.typicode.com/Balthazar33/myjson_json/inputs"
          )
          .then((response) => {
            self.isTyping = false;
            self.responseData = response.data;
            for (var i = 0; i < self.responseData.length; i++) {
              //loop over received response items
              for (var j = 0; j < self.responseData[i]["user_input"].length; j++) {
                //loop over user_input array for each
                if (self.responseData[i].user_input[j] == self.user_input.toLowerCase()) {
                  //Match is found

                  if (self.responseData[i].tag === 'vue_faqs') {
                    //If it's a preset question...
                    self.chats.push({
                      text: self.responseData[i].responses[index], //...provide preset response
                      user_or_bot: "bot",
                    });
                    self.match_found = self.responseData[i].responses[index];
                  } else {
                    var randomint = Math.floor(
                      Math.random() * self.responseData[i].responses.length
                    ); //random integer from 0 to length of response array
                    self.chats.push({
                      text: self.responseData[i].responses[randomint],
                      user_or_bot: "bot",
                    });
                    self.match_found = self.responseData[i].responses[randomint];
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
                text: "Sorry, didn't get that. Please contact",
                user_or_bot: "bot",
              });
            }
            self.scrollChatBoxToTop();
          });
        }
        else{
          this.isTyping = false;
          // console.log("responseData",this.responseData);
          axios.get("https://my-json-server.typicode.com/Balthazar33/myjson_json/inputs?tag=vue_faqs")
          .then(response=>{
            self.responseData = response.data;
          });
          // console.log("response",self.responseData)
          self.chats.push({
              //CHECK IF INPUT IS USER TYPED OR PRESET 
                text: faqClicked?  self.responseData[index]: self.responseData[0].responses[index],
                user_or_bot: "bot",
              });
          self.user_input = ""; //Clear input field
          self.scrollChatBoxToTop();
        }
      }
    },
     faq_clicked(index) {
       const self = this;
      axios.get("https://my-json-server.typicode.com/Balthazar33/myjson_json/inputs?tag=vue_faqs")
      .then(response=>{
        
        self.questions = response.data[0].user_input;
        self.responseData = response.data[0].responses;
        self.user_input = this.questions[index]; //Use preset faq question for chat
      self.hide_input = false; //Make faqs invisible
      self.send_Clicked(index,true);
      });
      
    },
    scrollChatBoxToTop(){
      //Make div scroll to top
      var front_card = document.getElementById("chats_wrapper");
      front_card.scrollTop = front_card.scrollHeight + 200;
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