<template>
  <div class="home">
    <picture-input ref="pictureInput" id="files-upload" width="600" height="600" margin="16"
      accept="image/jpeg,image/png" size="10" button-class="btn" :custom-strings="{
        upload: '<h1>Bummer!</h1>',
        drag: 'Drag a 😺 GIF or GTFO'
      }" @change="onChange">
    </picture-input>
    <br>
    <button v-if="showTrain" @click="recognize()">Recognize</button>
    <br>
    <div v-if="Object.entries(result).length>0">
      Words Found: {{result.words.length}} |
      Symbols: {{result.symbols.length}} |
      Paragraphs: {{result.paragraphs.length}} |
      Confidence:{{result.confidence}}%
      <br>
      <div v-if="result.confidence>50"><p>{{result.text}}</p></div>
      <!-- <br>
      {{result.text}} -->
    </div>
  </div>
</template>

<script>
  import Tesseract from 'tesseract.js';
  import PictureInput from 'vue-picture-input'
  export default {
    name: 'app',
    data() {
      return {
        result: {},
        showTrain:false
      }
    },
    components: {
      PictureInput
    },
    methods: {
      recognize() {
        const img = document.getElementById('files-upload');
        console.log(img.files[0])
        Tesseract
          .recognize(img.files[0])
          .then(result => {
            this.result = result
            console.log(result);
          });
      },
      onChange(image) {
        this.showTrain=true
        console.log('New picture selected!')
        if (image) {
          console.log('Picture loaded.')
          this.image = image
        } else {
          console.log('FileReader API not supported: use the <form>, Luke!')
        }
      }
    }
  }
</script>
<style lang="scss">
  .home {
    display: flex;
    flex-direction: column;

    img {
      max-width: 500px;
      max-height: 500px;
      margin: 0 auto;
    }
    button{
    background-color: #008CBA;
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    width: 320px;
    margin: 0 auto 
    }
  }
</style>