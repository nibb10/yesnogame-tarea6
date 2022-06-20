<template>
  <img v-if="img" :src="img" alt="bg">
  <div class="bg-dark"></div>

  <div class="indecision-container">
      <b-input-group 
      prepend="Hazme una pregunta">
        <b-form-input
            v-model="question"
            type="text">
        </b-form-input>
      </b-input-group>
      
      <p>Recuerda terminar la pregunta con un signo de interrogación</p>

      <div v-if="isValidQuestion">
          <h2>{{question}}</h2>
          <h1>{{answer}}</h1>
      </div>
  </div>
</template>

<script>
export default {

    data() {
        return {
            question: null,
            answer: null,
            img: null,
            isValidQuestion: false
        }
    },
    methods: {
        async getAnswer() {
            this.answer = 'Pensando...'

            const {answer, image} = await fetch('https://yesno.wtf/api').then(r => r.json())

            this.answer = answer === 'yes' ? 'Sí!' : 'No'
            this.img = image
        }
    },
    watch: {
        question(value){

            this.isValidQuestion = false

            if (!value.includes('?')) return

            this.isValidQuestion = true

            this.getAnswer()
        }
    }
}
</script>

<style scoped>

    img, .bg-dark {
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: black;
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }

    p {
        color: white;
        font-size: 20px;
        background-color: black;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }

</style>