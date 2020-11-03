<template>
<transition name="fade">
<div
          id="chats_wrapper"
          :class="hide_input===false?'card_scroll':''"
          v-if="hide_input===false"
        >
        <p class="isTyping" v-if="isTyping">typing...</p>
          <div class="chat_Container">
            <div class="chat clearfix" v-for="(entry,index) in chats" :key="'entry'+index">
              <!-- Check if user or bot, place accordingly -->
              <span v-if="entry.text.startsWith('Sorry')" :class="entry.user_or_bot=='bot'? 'bot_bubble':'user_bubble'">
                {{entry.text}} <a href="mailto:javascript:void(0);">support.</a>
              </span>
              <a :class="entry.user_or_bot=='bot'? 'bot_bubble':'user_bubble'" target="_blank" v-else-if="entry.text.endsWith('.html')" :href="entry.text">{{entry.text}}</a>
              <span v-else :class="entry.user_or_bot=='bot'? 'bot_bubble':'user_bubble'">{{entry.text}}</span>
            </div>
          </div>
        </div>
</transition>
    
</template>

<script>
export default {
    props:['chats','hide_input','isTyping']
}
</script>

<style scoped>
.isTyping {
    position: absolute;
    top: 50%;
    left: 45%;
    font-style:italic;
    color:rgb(109, 108, 108);
}
</style>