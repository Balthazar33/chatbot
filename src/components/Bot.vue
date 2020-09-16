<template>
  <div class="chatbot_container">
    <div class="bg_top_orange">
      <div class="iris_logo">
        <p>IRIS</p>
      </div>
      <div class="intro_txt">
        <span>
          Hello,
          <i class="fa fa-hand-paper-o"></i>
        </span>
        <span>I am Iris, a Virtual Assistant</span>
        <span>How may I help you?</span>
      </div>
    </div>
    <div class="front_bottom_white">
      <!--  CHAT section  -->
      <div id="appender_base">
        <div class="chat_Container clearfix">
          <div class="chats" v-for="(entry,index) in chats" :key="'entry'+index">
            <span :class="entry.user_or_bot=='bot'? 'bot_bubble':'user_bubble'">{{entry.text}}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- FAQS -->
    <div id="faqs" v-if="hide_input==true">
      <div class="faqs_title text-bold">
        <p>
          Frequently asked questions
          <i class="fa fa-question-circle-o"></i>
        </p>
      </div>

      <div class="faq_items" v-for="(item,index) in questions" :key="'item'+index">
        <span class="bullet_mark"></span>
        <span @click="faq_clicked(index)">{{item}}</span>
      </div>
    </div>

    <div class="ellipse">
      <svg
        id="boticon"
        width="36"
        height="30"
        viewBox="0 0 36 30"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M1.8 13.125H3.6V24.375H1.8C1.56354 24.3753 1.32935 24.327 1.11084 24.2329C0.892325 24.1387 0.693783 24.0006 0.526582 23.8265C0.35938 23.6523 0.226804 23.4455 0.136445 23.2179C0.0460864 22.9903 -0.000280023 22.7463 1.27228e-06 22.5V15C-0.000280023 14.7537 0.0460864 14.5097 0.136445 14.2821C0.226804 14.0545 0.35938 13.8477 0.526582 13.6735C0.693783 13.4994 0.892325 13.3613 1.11084 13.2671C1.32935 13.173 1.56354 13.1247 1.8 13.125ZM30.6 10.3125V26.25C30.5991 27.2443 30.2195 28.1975 29.5445 28.9006C28.8696 29.6036 27.9545 29.999 27 30H9C8.04551 29.999 7.13038 29.6036 6.45545 28.9006C5.78052 28.1975 5.40094 27.2443 5.4 26.25V10.3125C5.39981 9.69687 5.51607 9.08724 5.74215 8.51843C5.96823 7.94963 6.29969 7.43281 6.71759 6.99749C7.13549 6.56218 7.63165 6.21691 8.1777 5.98141C8.72375 5.74591 9.309 5.6248 9.9 5.625H16.2V1.875C16.2 1.37772 16.3896 0.900806 16.7272 0.549175C17.0648 0.197544 17.5226 0 18 0C18.4774 0 18.9352 0.197544 19.2728 0.549175C19.6104 0.900806 19.8 1.37772 19.8 1.875V5.625H26.1C26.691 5.6248 27.2763 5.74591 27.8223 5.98141C28.3684 6.21691 28.8645 6.56218 29.2824 6.99749C29.7003 7.43281 30.0318 7.94963 30.2578 8.51843C30.4839 9.08724 30.6002 9.69687 30.6 10.3125ZM14.85 15C14.85 14.5365 14.718 14.0833 14.4708 13.6979C14.2236 13.3125 13.8722 13.012 13.461 12.8347C13.0499 12.6573 12.5975 12.6109 12.161 12.7013C11.7246 12.7917 11.3237 13.0149 11.009 13.3427C10.6943 13.6705 10.48 14.0881 10.3932 14.5428C10.3064 14.9974 10.351 15.4686 10.5213 15.8969C10.6916 16.3252 10.98 16.6912 11.35 16.9488C11.72 17.2063 12.155 17.3438 12.6 17.3438C12.8955 17.3438 13.1881 17.2832 13.4611 17.1654C13.7341 17.0476 13.9821 16.875 14.191 16.6573C14.4 16.4397 14.5657 16.1813 14.6788 15.8969C14.7918 15.6126 14.85 15.3078 14.85 15ZM14.4 22.5H10.8V24.375H14.4V22.5ZM19.8 22.5H16.2V24.375H19.8V22.5ZM25.65 15C25.65 14.5365 25.518 14.0833 25.2708 13.6979C25.0236 13.3125 24.6722 13.012 24.261 12.8347C23.8499 12.6573 23.3975 12.6109 22.961 12.7013C22.5246 12.7917 22.1237 13.0149 21.809 13.3427C21.4943 13.6705 21.2801 14.0881 21.1932 14.5428C21.1064 14.9974 21.151 15.4686 21.3213 15.8969C21.4916 16.3252 21.78 16.6912 22.15 16.9488C22.52 17.2063 22.955 17.3438 23.4 17.3438C23.6955 17.3438 23.9881 17.2832 24.2611 17.1654C24.5341 17.0476 24.7821 16.875 24.991 16.6573C25.2 16.4397 25.3657 16.1813 25.4788 15.8969C25.5918 15.6126 25.65 15.3078 25.65 15ZM25.2 22.5H21.6V24.375H25.2V22.5ZM36 15V22.5C36.0003 22.7463 35.9539 22.9903 35.8636 23.2179C35.7732 23.4455 35.6406 23.6523 35.4734 23.8265C35.3062 24.0006 35.1077 24.1387 34.8892 24.2329C34.6706 24.327 34.4365 24.3753 34.2 24.375H32.4V13.125H34.2C34.4365 13.1247 34.6706 13.173 34.8892 13.2671C35.1077 13.3613 35.3062 13.4994 35.4734 13.6735C35.6406 13.8477 35.7732 14.0545 35.8636 14.2821C35.9539 14.5097 36.0003 14.7537 36 15Z"
          fill="white"
        />
      </svg>
    </div>

    <!-- INPUT SECTION -->
    <div class="input_box">
      <div class="start_conv" @click="hide_input=false" v-if="hide_input==true">
        <svg
          id="chat_icon"
          width="14"
          height="14"
          viewBox="0 0 14 14"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12.7696 2.07794H11.5391V1.66779C11.5391 0.989306 10.9871 0.437317 10.3086 0.437317H1.23047C0.551989 0.437317 0 0.989306 0 1.66779V8.2303C0 8.90878 0.551989 9.46077 1.23047 9.46077H2.46094V11.5116C2.46094 11.8775 2.90451 12.0582 3.16111 11.8016L3.8613 11.1014H8.4981L10.8389 13.4422C11.0956 13.699 11.5391 13.5188 11.5391 13.1522V11.1014H12.7695C13.448 11.1014 14 10.5494 14 9.87093V3.30842C14 2.62993 13.448 2.07794 12.7696 2.07794ZM3.28126 10.5214V9.05061C3.28126 8.8241 3.09762 8.64046 2.8711 8.64046H1.23047C1.00431 8.64046 0.820314 8.45646 0.820314 8.2303V1.66779C0.820314 1.44163 1.00431 1.25763 1.23047 1.25763H10.3086C10.5348 1.25763 10.7188 1.44163 10.7188 1.66779V8.2303C10.7188 8.45646 10.5348 8.64046 10.3086 8.64046H5.33204C5.22324 8.64046 5.11895 8.68369 5.04203 8.7606L3.28126 10.5214ZM13.1797 9.87093C13.1797 10.0971 12.9957 10.2811 12.7696 10.2811H11.1289C10.9024 10.2811 10.7188 10.4647 10.7188 10.6912V12.162L8.95799 10.4012C8.88108 10.3243 8.77676 10.2811 8.66799 10.2811H4.68161L5.50193 9.46077H10.3086C10.9871 9.46077 11.5391 8.90878 11.5391 8.2303V2.89826H12.7696C12.9957 2.89826 13.1797 3.08226 13.1797 3.30842V9.87093Z"
            fill="#2D2D2D"
          />
          <path
            d="M7.8477 2.89827H2.05081C1.8243 2.89827 1.64066 3.08191 1.64066 3.30843C1.64066 3.53494 1.8243 3.71858 2.05081 3.71858H7.8477C8.07421 3.71858 8.25786 3.53494 8.25786 3.30843C8.25786 3.08191 8.07421 2.89827 7.8477 2.89827Z"
            fill="#2D2D2D"
          />
          <path
            d="M9.48833 4.53889H2.05081C1.8243 4.53889 1.64066 4.72254 1.64066 4.94905C1.64066 5.17557 1.8243 5.35921 2.05081 5.35921H9.48833C9.71484 5.35921 9.89848 5.17557 9.89848 4.94905C9.89848 4.72254 9.71484 4.53889 9.48833 4.53889Z"
            fill="#2D2D2D"
          />
          <path
            d="M6.20707 6.17952H2.05081C1.8243 6.17952 1.64066 6.36316 1.64066 6.58968C1.64066 6.81619 1.8243 6.99983 2.05081 6.99983H6.20707C6.43359 6.99983 6.61723 6.81619 6.61723 6.58968C6.61723 6.36316 6.43359 6.17952 6.20707 6.17952Z"
            fill="#2D2D2D"
          />
        </svg>

        <p>Start a new conversation</p>
      </div>

      <div id="input_bottom" v-if="!hide_input">
        <input type="text" name="chat_input" v-model="user_input" />
        <a href="javascript:void(0);" @click="send_Clicked" class="send_btn">
          <i class="fa fa-paper-plane"></i>
        </a>

        <a href="javascript:void(0);" class="back_link" @click="reset_chat"><i class="fa fa-backward"></i></a>
      </div>
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
      botReply: [],
      user_entry: [],
      nomatch: 0,
      // chatResponse: [],
      chats: [],
    };
  },
  methods: {
    send_Clicked(index) {
      // document.getElementById("faqs").style.display = "none";

      var self = this;
      if (self.user_input != "") {
        // self.user_entry.push(self.user_input);
        self.chats.push({
          text: self.user_input,
          user_or_bot: "user",
        });

        axios
          .get("https://my-json-server.typicode.com/Balthazar33/myjson_json/inputs")
          .then((response) => {
            this.data = response.data;
            for (var i = 0; i < self.data.length; i++) {
              //loop over data items
              for (var j = 0; j < self.data[i]["user_input"].length; j++) {
                //loop over user input array for each
                if (self.data[i].user_input[j] == self.user_input) {
                  

                  if (self.data[i].tag == "finance") { //If it is a question of finance category...
                    self.chats.push({
                      text: self.data[i].responses[index], //...provide accurate response
                      user_or_bot: "bot",
                    });
                  } else {
                    var randomint = Math.floor(
                      Math.random() * self.data[i].responses.length
                    ); //random integer from 0 to length of response array
                    // self.botReply.push(self.data[i].responses[randomint]); //get a random response from preset ones and save in responses
                    self.chats.push({
                      text: self.data[i].responses[randomint],
                      user_or_bot: "bot",
                    });
                    self.nomatch = 0;
                  }
                  
                }
              }
            }
            self.user_input = "";
          });
      }
      if (self.nomatch === 1) {
        self.chats.push({
          text: "Sorry, didn't get that.",
          user_or_bot: "bot",
        });
      }
    },
    faq_clicked(index) {
      this.user_input = this.questions[index];  //Use preset faq question for chat
      this.hide_input = false;
      this.send_Clicked(index);
    },
    reset_chat(){
      this.hide_input = true;  //Go back to start page
      this.chats = [];
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.relative {
  position: relative;
}

::-webkit-scrollbar {
  display: none;
}

.float_left {
  float: left;
}
.float_right {
  float: right;
}

.text-bold {
  font-weight: 600;
}
.faqs_title i {
  color: rgba(255, 118, 0, 0.8);
  position: absolute;
  right: -20px;
  font-weight: 600;
  font-size: 1.2em;
}
.bullet_mark {
  position: absolute;
  width: 8px;
  height: 8px;
  background: rgba(255, 118, 0, 0.8);
  left: 10px;
  top: 20px;
  border-radius: 50%;
}
.chatbot_container {
  position: relative;
  width: 416px;
  height: 629px;
  background: #ffffff;
  box-shadow: 0px 0px 50px rgba(0, 0, 0, 0.08);
  border-radius: 18px;
  margin: 0 auto;
}
.bg_top_orange {
  position: absolute;
  width: 416px;
  height: 278px;
  left: 0px;
  top: -1px;
  border-radius:15px 15px 0 0;
  background: linear-gradient(123.75deg, #f5821f 1.87%, #ff5c00 120.46%);
}
.iris_logo {
  position: absolute;
  width: 56px;
  height: 36px;
  left: 56px;
  top: -10px;
  font-family: inherit;
  font-style: normal;
  font-weight: bold;
  font-size: 30px;
  line-height: 36px;

  color: #ffffff;
}

.intro_txt {
  position: absolute;
  width: 245px;
  height: 46px;
  left: 56px;
  top: 70px;
  font-family: Silka;
  font-style: normal;
  font-weight: 500;
  font-size: 18px;
  line-height: 23px;
  color: #ffffff;
}
.intro_txt span {
  text-align: left;
  display: block;
}

.front_bottom_white {
  position: absolute;
  width: 390px;
  height: 370px;
  overflow-y: scroll;
  left: 13px;
  top: 199px;
  background: #ffffff;
  border: 1px solid #e4e4e4;
  box-sizing: border-box;
  box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.05);
  border-radius: 8px;
}

.ellipse {
  position: absolute;
  width: 67px;
  height: 67px;
  left: 38px;
  top: 165px;
  border-radius: 50%;
  background: linear-gradient(180deg, #ff7600 0%, #ff5c00 100%);
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.15);
}
#boticon {
  position: absolute;
  top: 15px;
  right: 15px;
}

.faqs_title {
  position: absolute;
  width: 194px;
  height: 17px;
  left: 39px;
  top: 249px;
  font-family: inherit;
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  line-height: 17px;
  color: #2d2d2d;
}

.faq_items {
  position: relative;
  width: 340px;
  height: 50px;
  left: 38px;
  background: #ffffff;
  border: 1px solid #e1e1e1;
  box-sizing: border-box;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.05);
  border-radius: 8px;
  padding: 15px 22px;
  font-size: 0.9em;
  text-align: left;
  cursor: pointer;
}

.faq_items:nth-child(2) {
  top: 300px;
}
.faq_items:nth-child(3) {
  top: 310px;
}
.faq_items:nth-child(4) {
  top: 320px;
}
.faq_items:nth-child(5) {
  top: 330px;
}

/* INPUT SECTION */
.input_box {
  position: absolute;
  width: 268px;
  height: 44px;
  cursor: pointer;
  left: 74px;
  top: 580px;
  background: #ffffff;
  border: 1px solid #d1d1d1;
  box-sizing: border-box;
  box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.1);
  border-radius: 100px;
}
.start_conv p {
  padding: 10px 0;
  margin: 0;
}
#chat_icon {
  position: absolute;
  left: 25px;
  top: 12px;
}
#input_bottom {
  position: relative;
}
#input_bottom input {
  width: 80%;
  height: 20px;
  padding: 10px;
  border: none;
}
#input_bottom input:focus {
  outline: none;
}
.send_btn {
  position: absolute;
  top: 10px;
  right: 10px;
  color: #000;
}

.back_link {
  position: absolute;
    left: -50px;
    top: 10px;
    color: #000;
}

/* RESPONSE AND INPUT SECTION */
#appender_base {
  width: 100%;
  height: 1px;
  background: #fff;
  position: absolute;
  top: 100px;
}

.chats span {
  border: 1px solid;
  display: block;
  width: 100px;
  padding: 5px 20px;
  border-radius: 5px 15px 5px 15px;
  clear: both;
  margin-bottom: 10px;
}
.user_bubble {
  float: right;
  background: powderblue;
  margin-right: 10px;
}
.bot_bubble {
  background: lightgreen;
  float: left;
  margin-left: 10px;
}
</style>
