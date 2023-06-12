<template>
  <v-container>
      <div class="header card ">

              <h1 class="mt-3 text-center">Hızlı Yazma Yarışması</h1>
              <h6 class="mt-3 text-center text-danger">Ne Kadar Hızlı Yazıyorsun? </h6>
              <div>
                 Doğru Sayısı : {{ trueNumber }}
                 <br>
                 Yanlış Sayısı : {{ falseNumber }}
              </div>
              <div v-if="time === 0" class="alert alert-warning mt-3" role="alert">
                    <h1 class="fw-bold text-center">Oyun Bitti</h1>
              </div>
              <div v-else>
              <div  class="card mt-3">
                  <div  class="card-body">
                      <span  v-for="(word,key) in words" :key="key"
                    v-bind:class="key!=0 || cssCount"
                      class="words ml-2">
                          {{ word }}
                      </span>
                      
                  </div>
              </div>
          <div class="card d-flex">
              <div class="card-body">
                  <div class="input-group">
                    
                  <input  v-model="inputWord" type="text" class="form-control">
                  <div class="input-group-append">
                      <button class="btn btn-outline-secondary ms-2" type="button">{{ time }}.sn</button>
                      <button class="btn btn-outline-secondary ms-2" type="button">Yenile</button>
                  </div>
                  </div>
              </div>
             
          </div> 
        </div>
      </div>
  </v-container>
</template>

<script>
import axios from 'axios'
  export default{
    data(){
      return{
        words:[],
        inputWord: null,
        trueWord: true,
        trueNumber: 0,
        falseNumber:0,
        time:3,
        interval: false,
        isRunnig: false 
      }
    },
    watch:{
       inputWord(input){
        if(!this.isRunnig)this.setTimeOut()
        const wordArray= this.words[0].slice(0,input.length)
        const spaceWord= input.replace(" ","")
        this.trueWord = spaceWord == wordArray
        
        if(input.indexOf(" ")!== -1){
            this.trueWord ? this.trueNumber++ : this.falseNumber++
            this.inputWord = ""
           this.words.splice(0,1)
        }
       }
    },
    created(){
      axios.get('http://localhost:3000/words').then(get_response => {
        this.words = get_response?.data || []
      })
    },
    computed:{
        cssCount(){
          return this.trueWord ? 'cssWord' : 'cssWord bg-danger'
        }
    },
    methods:{
        setTimeOut(){
          this.isRunnig=true
          this.interval = setInterval(this.timer, 1000)
        },
        timer(){
          if(this.time === 0){
            clearInterval(this.interval)
            
            return
          }
          this.time--
        },
        
        
    }
  }
</script>

