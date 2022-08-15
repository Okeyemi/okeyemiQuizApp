<template>
<main class="app">
  <h1>The Quiz</h1>

  <section class="quiz" v-if="!quizCompleted">
     <div class="quiz-info">
    <span class="question">{{getCurrentQuestion.question}}</span>
    <span class="score">Score{{score}}/{{questions.length}} </span>
  </div>

  <div class="options">
    <label
     v-for="(option, index) in getCurrentQuestion.options"
     :key="index"
     :for="'option' + index"
     :class=" `option ${
       getCurrentQuestion.selected == index
       ? index == getCurrentQuestion.answer
       ?'correct'
       :'wrong'
      : ''

    } ${
      getCurrentQuestion.selected !=null &&
      index !=getCurrentQuestion.selected
      ?'disabled'
      :''
    }`">

      <input type="radio"
      :id="'option' + index"
      :name="getCurrentQuestion.index"
      :value="index"
      v-model="getCurrentQuestion.selected"
      :disabled="getCurrentQuestion.selected"
      @change="SetAnswer()"
    />

      <span>{{ option }}</span>
    </label>
  </div>

  <button
  @click="NextQuestion"
  :disabled="!getCurrentQuestion.selected">
  {{
    getCurrentQuestion.index==questions.length -1
    ? 'Finish'
    : 'NextQuestion'
  }}
  </button>
<footer class="home">
  Powered by OKEYEMI CREATIVE
</footer>
  </section>
  <section v-else>
    <h2>You have finished the quiz</h2>
    <p>Your score is {{score}} / {{ questions.length }}</p>
    <footer class="about">
      <P>Powered by OKEYEMI CREATIVE <br>
      <a href="mailto:okeyemitunde@gmail.com">Contact Us</a></P>
    </footer>
  </section>
 
</main>
</template>

<script>
import{ref, computed, defineComponent} from 'vue'

export default defineComponent({
  setup() {

    const questions=ref([
    {
      question:'what is vue js?',
      answer:0,
      options:[
        'A front end framework',
        'A library',
        'state management library'
      ],
      selected:null
    },
    {
      question:'what is vuex?',
      answer:2,
      options:[
        'A front end framework',
        'A library',
        'An ice Cream maker'
      ],
      selected:null
    },
    {
      question:'what is vue js?',
      answer:0,
      options:[
        'A front end framework',
        'A library',
        'An ice Cream maker'
      ],
    selected:null
    },
    ])
    const quizCompleted=ref(false)
    const currentQuestion=ref(0)
    const score = computed( () => {
      let value=0
      questions.value.map(q=>{
        if(q.selected != null && q.answer==q.selected) 
        value++
        }
      })
      return value
    })
    const getCurrentQuestion = computed(()=>{
      let question=questions.value[currentQuestion.value]
      question.index = currentQuestion.value
      return question
    })

    const SetAnswer= evt =>{
      questions.value[currentQuestion.value].selected=evt.target.value
      evt.target.value=null
    }
    const NextQuestion =()=>{
      if (currentQuestion.value < questions.value.length -1){
        currentQuestion.value++
      }else{
        quizCompleted.value=true
      }
    }
  
  return {
    getCurrentQuestion, questions, score, quizCompleted, SetAnswer, NextQuestion
  }
  }
})
</script>


<style>
*{
  margin: 0;
  padding: 0%;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
body{
  background-color: #271c36;
  color: #fff;
}
.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz{
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}
.quiz-info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.quiz-info .question{
  color: #8f8f8f;
  font-size: 1.48rem;
}
.quiz-info .score{
  color: #fff;
  font-size: 1.25rem;
}
.options{
  margin-bottom: 1rem;
}
.option{
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.options:hover{
  background-color: #2d213f;
}
.option.correct{
  background-color: #2cce7d;
}
.option.wrong{
  background-color:#ff5a5f;
  }
  .option:last-of-type{
    margin-bottom: 0;
  }
  .option.disabled{
    opacity: 0.5;
  }
  .option input{
    display: none;
  }
button{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}
button:disabled{
  opacity: 0.5;
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p{
  color: #8f8f8f;
  font-size: 1.5rem;
  text-align: center;
}
.about{
  text-align: center;
  padding:3px;
margin: 20px;

}
.about p{
  color: #fff;
}
.home{
  text-align: right;
  font-family:'Times New Roman';
}
</style>

