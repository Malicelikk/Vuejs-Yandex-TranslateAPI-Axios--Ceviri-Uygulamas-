<template>

   <div class="container">
      <h1 class="text-center">Vue-Translator</h1>
      <h5 class="text-center">Vue.js ile Translator</h5>
      <br>

      <div class="row">
      
          <div class="col-md-12 text-center">

             <TranslateForm 
             @translatedEvent="translatedText = $event"
             @historyEvent="history.push($event)"/>  <!--eventle geleni al diziye pushla-->

             <h3 class="text-success text-center">{{translatedText}}</h3>

             <SearcHistory :history="history"/> <!--gönder-->
          </div>
      </div>

   </div>
 
</template>

<script>

   import TranslateForm from "./components/TranslateForm"
   import SearcHistory from "./components/SearchHistory"

   export default {

      components : {
         TranslateForm,
         SearcHistory
      },
      data(){
         return {
            translatedText : '',
            history : []  // history arrayini :history propsuyla searcHistory componentine gönder
         }
      }
  
   }
</script>


<style>

body{
   background-color: #dedede;
}

</style>
