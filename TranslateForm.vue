<template>

        <form @submit.prevent="translateIt" class="well">

                <textarea v-model="translateText" cols="30" rows="5" class="form-control" placeholder="Çevirmek istediğiniz kelime/cümle yazınız."></textarea>

                <select v-model="translateTo" class="form-control">
                    <option v-for="(value,key) in languages" :value="key">{{value}}</option>
                </select>
                <br>

                <div class="text-left" v-if="detectedLang">
                    <strong>Tespit Edilen Dil : {{ detectedLang }}</strong> 
                </div>
                <br>

                <button type="submit" class="btn btn-primary btn-block">Çevir Gelsin!</button>

        </form>
           
 
</template>

<script>

import axios from "axios"

    export default {

        data(){
            return {
                translateText : '',
                translateTo : '',
                languages : {} ,
                detectedLang: ''
            }
        },
        methods : {
            translateIt(){

                let url = "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20130922T110455Z.4a9208e68c61a760.f819c1db302ba637c2bea1befa4db9f784e9fbb8&text=" + this.translateText + "&lang=" + this.translateTo

                axios.get(url)
                .then(response => {
                    this.detectedLang = this.languages[this.translateTo]
                    console.log(response)

                    this.$emit("translatedEvent", response.data.text[0])  // childdan parenta çeviriyi gönder

                    let history = {
                        from : this.languages[response.data.lang.split("-")[0]],
                        to : this.detectedLang,
                        translateText: this.translateText,
                        translatedText: response.data.text[0]
                    }

                    this.$emit("historyEvent", history); // history objesini childdan parenta gönder

                    this.translateTo = ''
                    this.translateText = ''

                }).catch(e => console.log(e))
                
                //alert(this.translateText);
                //alert(this.translateTo);

            }
        },
        created(){
            axios.get("https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20130922T110455Z.4a9208e68c61a760.f819c1db302ba637c2bea1befa4db9f784e9fbb8&ui=tr")
            .then(response => {
                console.log(response)
                this.languages = response.data.langs  // gelen datadan langs ı languages objesine attık
            })
            .catch(e => console.log(e))
        }
  
    }

</script>


<style>

</style>
