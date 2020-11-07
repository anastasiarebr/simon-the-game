<template>
  <div class="simongame">    
    <audio ref="sound1" src="https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/1.mp3"></audio>    
    <audio ref="sound2" src="https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/2.mp3"></audio>    
    <audio ref="sound3" src="https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/3.mp3"></audio>    
    <audio ref="sound4" src="https://raw.githubusercontent.com/kellyk/javascript-simon/master/sounds/4.mp3"></audio>    
    
    <h1 class="title">Simon Says</h1>  

    <div class="game">      
      <div class="game__circle">        
        <div class="game__button"         
        :class="{highlight: dBlue}"         
        id="blue"         
        v-on:click="input(1)"></div>        
        <div class="game__button"         
        :class="{highlight: dPink}"         
        id="pink"         
        @click="input(2)"></div>        
        <div class="game__button"         
        :class="{highlight: dYellow}"         
        id="yellow"        
         @click="input(3)"></div>        
         <div class="game__button"         
         :class="{highlight: dGreen}"         
         id="green"         
         @click="input(4)"></div>      
    </div>  

        <div class="game__options">        
          <div class="game__rounds"> 
            <h2>Round: {{count}}</h2> 
          </div>        
          <button class="game__start" @click="start">Start</button>        
             
          <p class="error" 
          v-if="showError">Sorry, you lost! Your score: {{count - 1}}</p>        
          <div class="game__levels" >
            <h2>Levels</h2> 

            <br><input type="radio" value=1500 v-model="level" checked> Easy          
            <br><input type="radio" value=1000 v-model="level"> Normal          
            <br><input type="radio" value=400 v-model="level"> Hard       
          </div>      
        </div>    
    </div>  

  </div>  
</template>    

<script>  
export default {  
  data(){    
    return {      
      started: false,     
      count: 0,      
      userInput: [],      
      series: [],       
      playingSeries: false,      
      dBlue: false,      
      dPink: false,      
      dYellow: false,      
      dGreen: false,      
      allowInput: false,      
      showError: false,      
      level: 1500    
      }  
  },  
  methods: {    
    reset(){      
      this.started = false;      
      this.count = 0;      
      this.userInput = [];      
      this.series = [];      
      this.playingSeries = false;      
      this.dBlue = false;      
      this.dPink = false;      
      this.dYellow = false;      
      this.dGreen = false;      
      this.allowInput = false;      
      this.showError = false;      
      this.level = 1500;    
    },    

    input(tone) {      
      if (!this.allowInput) {        
        this.playTone(tone);      
        } else {        
          this.playTone(tone);        
          this.userInput.push(tone);        
          if (this.userInput[this.userInput.length - 1] != this.series[this.userInput.length - 1]) {        
            this.userInput = [];        
            this.allowInput = false;        
            let self = this;        
            this.showError = true;        
            setTimeout(this.reset, 1000);        
            setTimeout(this.start, 400);        
            setTimeout(function() { self.showError = false; self.playSeries() }, 1000);      }      
            else if (this.userInput.length == this.series.length) {        
              let self = this;        
              this.userInput = [];          
              setTimeout(function() {             
                self.addTone();            
                self.playSeries();          
                }, 1000)      
              }     
        }   
    }, 

    start() {      
      if (this.count == 0) {        
        this.started = true;        
        this.addTone();        
        this.playSeries();      
        }    
    },   

    addTone() {      
      this.count++;      
      this.series.push(this.randomTone());    
    },    

    playSeries() {     
      this.allowInput = false;      
      this.playingSeries = true;      
      let self = this;      
      let oldDelay = Number(this.level);      
      let delay = oldDelay;      
      this.series.forEach(function(tone, index, array) {        
        if (index == array.length - 1){          
          setTimeout(function() {             
            if (self.started) {            
              self.playTone(tone);            
              self.allowInput = true;            
              self.playingSeries = false;            
            }          
          }, delay);        
        }else {        
          setTimeout(function() { self.playTone(tone) }, delay);        
        }        
        delay+=oldDelay;              
      })      
      this.playingSeries = false;    
    }, 

    clearHighlights() {      
      this.dGreen = false;      
      this.dPink = false;      
      this.dYellow = false;      
      this.dBlue = false;    
    },    

    playTone(tone) {              
      switch (tone) {        
        case 1:          
          this.$refs.sound1.pause();          
          this.$refs.sound1.currentTime = 0; 
          this.$refs.sound1.play();          
          this.dBlue = true;          
          break;        
        case 2:          
          this.$refs.sound2.pause();          
          this.$refs.sound2.currentTime = 0;          
          this.$refs.sound2.play();          
          this.dPink = true;          
          break;        
        case 3:          
          this.$refs.sound3.pause();          
          this.$refs.sound3.currentTime = 0;          
          this.$refs.sound3.play();          
          this.dYellow = true;          
          break;        
        case 4:          
          this.$refs.sound4.pause();          
          this.$refs.sound4.currentTime = 0;          
          this.$refs.sound4.play();          
          this.dGreen = true;          
          break;      
      }         
        setTimeout(this.clearHighlights, 750)    
    },    
    
    stopTones() {      
      this.$refs.sound1.pause();      
      this.$refs.sound2.pause();      
      this.$refs.sound3.pause();      
      this.$refs.sound4.pause();      
      this.$refs.sound1.currentTime = 0;      
      this.$refs.sound2.currentTime = 0;      
      this.$refs.sound3.currentTime = 0;      
      this.$refs.sound4.currentTime = 0;    
    },    
    
    randomTone() {      
      return Math.floor(Math.random() * 4) + 1;    
    }  
  }  
}   

</script>  


<style lang="sass">  
@import 'components/style.sass'  
</style>  
