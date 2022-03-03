<template>
  <img v-if="imageTemplate" :src="imageTemplate" alt="">
  <div class="fondo-oscuro"></div>

  <div class="contenedor-question">
    <input type="text" placeholder="Realice una Pregunta" v-model="question"/>
    <br>
    <p>Realiza la pregunta, no olvides terminar con el signo "?"</p>

    <div>
      <h2>{{question}}</h2>
      <h1>{{ answerTemplate}}</h1>
    </div>
  </div>

</template>

<script>
export default {
  data() {
    return {
      question: '',
      answerTemplate: '',
      imageTemplate: '',
    }
  },
  methods: {
    async getAnswer() {
      try {
        const data = await fetch('https://yesno.wtf/api');
        const answerApi = await data.json();
        // console.log(answerApi);
        const { answer, image } = answerApi;
        // console.log(answer);
        // console.log(image);
        this.answerTemplate = answer == 'yes'? 'SI!!': 'NO!!';
        this.imageTemplate = image;
      } catch (error) {
        console.log(error);
        throw error;
      }
    }
  },
  watch: {
    question(value, oldValue) {
      console.log(value.includes('?')); // true

      if(!value.includes('?')) return;
      this.getAnswer();
      
    }
  }

}
</script>

<style scoped>
  img, .fondo-oscuro {
    height: 100vh;
    position: fixed;
    left: 0px;
    top: 0px;
    width: 100vw;
    max-height: 100%;
    max-width: 100%;
  }


  .fondo-oscuro {
    background: rgba(0,0,0,0.4);
  }

  .contenedor-question {
    position: relative;
    z-index: 1;
  }

  input {
    margin-top: 40px;
    width: 250px;
    padding: 10px 15px;
    border-radius: 5px;
    border: none;
  }

  input:focus {
    outline: none;
  }

  p {
    color: white;
    font-size: 24px;
    margin-top: 10px;
  }

  h1, h2 {
    color: white;
  }

  h2 {
    margin-top: 170px;
  }
</style>