<template>
    <div class="open_chatbot_wrapper">
        <div class="open_chatbot ellipse" :class="pulsate?'pulsate':''"  @click="open_chatbot">
        <i class="fa fa-comments"></i>
        <!-- <img src="@/assets/images/Vector.svg" alt /> -->
      </div>
    </div>
    
</template>

<script>
export default {
    data(){
        return{
            pulsate:true
        }
    },
    methods:{
        open_chatbot(){
            this.$emit('open-chatbot');
            this.pulsate = !this.pulsate;
        }
    }
}
</script>

<style scoped>
.open_chatbot_wrapper{
    position: fixed;
    right: 100px;
    bottom:100px;
}
.ellipse.pulsate:after {
content: "";
    width: 100%;
    height: 100%;
    border-radius: 50%;
    position: absolute;
    z-index:-1;
    background: #dd1507;
    left: 0;
    top: 0;
    -webkit-animation: pulsate 1.5s   forwards;
    animation:pulsate 1.5s  infinite;
}



@keyframes pulsate {
    0%{transform: scale(1);opacity: 1;}
    100%{transform: scale(1.2);opacity: 0;}
}



</style>