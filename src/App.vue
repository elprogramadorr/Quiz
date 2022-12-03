
<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  {
    question: "Que es VueJS?",
    answer: 2,
    options: [
      "Es un programador.",
      "Es un lenguaje de programación",
      "Un framework de front-end"
    ],
    selected: null
  },{
    question: "Que es Vuex?",
    answer: 1,
    options: [
      "Es una ave",
      "Es una libreria de Gestión de Estados",
      "Una DBMS",
    ],
    selected: null
  },{
    question: "Quien soy?",
    answer: 0,
    options: [
      "No se",
      "Una Persona",
      "Un programador",
    ],
    selected: null
  }
]
)
const quizCompleted=ref(false)
const currentQuestion=ref(0)
const score=computed( ()=>{
  let value = 0
  questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			value++
		}
	})
	return value
  return value
}
)

const getCurrentQuestion=computed( ()=>{
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question; 
}
)

const SetAnswer =  evento =>{
  questions.value[currentQuestion.value].selected = evento.target.value
  evt.target.value=null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
  <link rel="stylesheet" href="style.css">
  <main class="app">
    <h1>Questionario🦁</h1>

    <section class="class" v-if="!quizCompleted">
      <div class="quizinfo">
        <span class="question">{{getCurrentQuestion.question}}</span>
        <span class="score"> Puntaje {{score}} / {{questions.length}}</span>
      </div>
      <div class="options">
        <label v-for="(option,index) in getCurrentQuestion.options" 
          :class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					}
					}${
            getCurrentQuestion.selected!=null && index!=getCurrentQuestion.selected 
              ? 'disabled'
              : ''
          }`">
          <input
           type="radio" :name="getCurrentQuestion.index"
            :value ="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer">
          <span>{{option}}</span>
        </label>
      </div>
      <button class="NextQuestion" 
      @click="NextQuestion"
      :disabled="!getCurrentQuestion.selected">
      {{
        getCurrentQuestion.index==questions.length-1
          ?'Terminar'
          :getCurrentQuestion.selected==null
            ? 'Selecciona una opción'
            : 'Siguiente'
      }}</button>
    </section>
    <section v-else>
      <h2>Gracias por Terminar el Questionario</h2>
      <h3>Tu puntaje es {{ score }} / {{questions.length}}</h3>
      <h3 v-show="(score==3)">Puntaje Perfecto 🎉</h3>
    </section>
  </main>
</template>

<style>
* { 
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}
body {
  background-color: rgba(41, 67, 101, 0.948);
  color:rgb(183, 199, 100);
}
.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}
h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}
.quiz {
	background-color: #382a4b;
	padding: 1rem;
	width: 100%;
	max-width: 640px;
}
.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}
.quiz-info .question {
	color: #8F8F8F;
	font-size: 1.25rem;
}
.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}
.options {
	margin-bottom: 1rem;
}
.option {
	padding: 1rem;
	display: block;
	background-color: #271c36;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}
.option:hover {
	background-color: #2d213f;
}
.option.correct {
	background-color: #2cce7d;
}
.option.wrong {
	background-color: #ff5a5f;
}
.option:last-of-type {
	margin-bottom: 0;
}
.option.disabled {
	opacity: 0.5;
}
.option input {
	display: none;
}
button {
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
button:disabled {
	opacity: 0.5;
}
h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}
p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}
</style>