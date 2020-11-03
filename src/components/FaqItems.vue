<template>
     <div class="faq_items">
            <div
              class="faq_item"
              v-for="(item,index) in questions"
              :key="'item'+index"
              @click="faq_clicked(index)"
            >
             
              <i class="fa fa-arrow-circle-right"></i><p>{{item}}</p>
            </div>
          </div>
</template>

<script>
export default {
    props:['questions'],
    methods:{
        faq_clicked(index){
            this.$emit('faq-clicked',index);
        }
    }
}
</script>


<style scoped>

</style>