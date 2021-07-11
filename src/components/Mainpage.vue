<template>
  <div class="d-flex justify-content-center">
    <!-- Result Score-->
    <div v-if="showScore">
        <b-card
        title="Results"
        style="max-width: 20rem;"
        >
        You Scored <b>{{score}}</b> of <b>{{questions.length}} </b>
        <br>
        <br>
        <u>Results are registered</u>
        <br>
        <br>
        <router-link type="submit" class="btn btn-dark btn-lg btn-block" to="/login" >Exit</router-link>

        </b-card>
    </div>
    <!-- Exam Page-->
    <div class="card-q" v-else>
    <span v-if="!startQuiz">
    <b-card
    no-body
    title="Online Exam"
    style="max-width: 30rem;"
    img-src="https://www.cardiffboxoffice.com/asset/Event/17857/1quiz_header_2.jpg"
    img-width="50px"
    img-alt="Image"
    img-top >
        <b-card-text>
            <center> General Ability=<b>10 Question</b></center>
            <center> Exam Time=<b>30 sec For Each Question</b></center> 
        </b-card-text>
      <b-button @click="startQuizFunc()">Start Exam!</b-button>
    </b-card>
    </span>
    <span v-else>
    <b-card
    title="General Aptitude Test"
    style="max-width: 30rem;"
    class="mb-2"
  >
  <!-- Questions Number-->
   <b-card-text>
      Question No.{{currentQuestion + 1}} of {{questions.length}}
    </b-card-text>
    <br>
   <b-progress
        variant="warning"
        :max="30"
        :value="countDown"
        height="5px"
      ></b-progress>
  
     <b-card-text>
      <span style="font-size: 40px;"><strong>{{countDown}} </strong></span>
    </b-card-text>
    <b-card-text>
      {{questions[currentQuestion].questionText}}
    </b-card-text>
    <div class="answer-section">
    <b-button :key="index" v-for="(option, index) in questions[currentQuestion].answerOptions" @click="handleAnswerClick(option.isCorrect)" class="ans-option-btn" variant="primary">{{option.answerText}}</b-button>
    </div>
  </b-card>
    </span>
  </div>
  </div>
</template>

<script>
//Questions
export default {
    data(){
        return {
            currentQuestion: 0,
            showScore: false,
            score:0,
            countDown : 30,
            timer:null,
            startQuiz: false,
            questions : [
		{
			questionText: 'When did The Avengers come out?',
			answerOptions: [
				{ answerText: 'May 4, 2012', isCorrect: false },
        { answerText: 'May 3, 2013', isCorrect: false },
				{ answerText: 'May 2, 2008', isCorrect: true },
				{ answerText: 'April 4, 2014', isCorrect: false },
			],
		},
		{
			questionText: 'Who is the creator of vueJS ?',
			answerOptions: [
				{ answerText: 'Jeff Bezos', isCorrect: false },
				{ answerText: 'Elon Musk', isCorrect: false },
				{ answerText: 'Evan You', isCorrect: true },
				{ answerText: 'Tony Stark', isCorrect: false },
			],
		},
		{
			questionText: 'Vue is used in the backend. - True or False?',
			answerOptions: [
				{ answerText: 'True', isCorrect: false },
				{ answerText: 'False', isCorrect: true },
			],
		},
		{
			questionText: 'What is 48,879 in hexidecimal?',
			answerOptions: [
				{ answerText: '0x12D591', isCorrect: false },
				{ answerText: '0xDEAD', isCorrect: false },
				{ answerText: '0xBEEF', isCorrect: false },
				{ answerText: '0x18C1', isCorrect: true },
			],
        },
        {
			questionText: 'Is vue an OpenSource Library?',
			answerOptions: [
				{ answerText: 'True', isCorrect: true },
				{ answerText: 'False', isCorrect: false },
			],
        },
         {
			questionText: 'What is 70 degrees Fahrenheit in Celsius?',
			answerOptions: [
				{ answerText: '21.1111', isCorrect: false },
				{ answerText: '18.8889', isCorrect: false },
        { answerText: '20', isCorrect: true },
				{ answerText: '158', isCorrect: false },
			],
        },
         {
			questionText: 'What is the capital of Spain?',
			answerOptions: [
				{ answerText: 'Berlin', isCorrect: false },
				{ answerText: 'San Juan', isCorrect: false },
        { answerText: 'Madrid', isCorrect: true },
				{ answerText: 'Barcelona', isCorrect: false },
			],
        },
         {
			questionText: 'Who is the founder of Apple Co.',
			answerOptions: [
					{ answerText: 'Bill Gates', isCorrect: false },
				{ answerText: 'Mark Zuckerberg', isCorrect: false },
        { answerText: 'Elon Musk', isCorrect: false },
				{ answerText: 'Steve Jobs', isCorrect: true},
			],
        },
        {
			questionText: 'Which of the following is not a Javascript Framework?',
			answerOptions: [
				{ answerText: 'Vue', isCorrect: false },
				{ answerText: 'Node', isCorrect: false },
				{ answerText: 'React', isCorrect: false },
				{ answerText: 'Laravel', isCorrect: true },
			],
        },
        {
			questionText: 'Which is not an operating system?',
			answerOptions: [
				{ answerText: 'Angular', isCorrect: true },
				{ answerText: 'Windows', isCorrect: false },
				{ answerText: 'Linux', isCorrect: false },
				{ answerText: 'MacOS', isCorrect: false },
			],
		},
    ],
    
        }
    },
    methods:{
        startQuizFunc(){
            this.startQuiz = true
            this.countDownTimer()
        },
        handleAnswerClick(isCorrect){
            clearTimeout(this.timer);
            let nextQuestion = this.currentQuestion + 1;
            if(isCorrect){
                this.score = this.score + 1;
            }
            if(nextQuestion < this.questions.length){
            this.currentQuestion = nextQuestion;
            // this.$store.state.questionAttended = this.currentQuestion;
            // localStorage.setItem('qattended', this.currentQuestion)
            this.countDown = 30;
            this.countDownTimer();
            }
            else{
                // localStorage.removeItem('qattended')
                this.showScore = true;
                // localStorage.setItem('testComplete',this.showScore)
            }
        },
        countDownTimer() {
                if(this.countDown > 0) {
                    this.timer = setTimeout(() => {
                        this.countDown -= 1
                        this.countDownTimer()
                    }, 1000)
                }
                else{
                    this.handleAnswerClick(false)
                }
            }
    },
    next: function() {
         if (this.questionIndex < this.quiz.questions.length)
            this.questionIndex++;
      },
     created() {
        //  alert(this.$store.state.questionAttended)
        //    this.showScore = localStorage.getItem('testComplete') || false
        //    this.currentQuestion = localStorage.getItem('qattended') || 0
        //    this.countDownTimer()
        //    this.fetchQuiz()
        }
    
}
</script>

<style scoped>
.card{
    min-width: 100%;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 10px 10px 42px 0px rgba(0, 0, 0, 0.75);
}
.card-q{
    max-width: 40rem;
    
}
.ans-option-btn{
    min-width: 50%;
    font-size: 16px;
    color: #ffffff;
    align-items: center;
    cursor: pointer;
    margin-bottom: 5px;
}
.answer-section {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.timer-text {
  background: rgb(230, 153, 12);
  padding: 15px;
  margin-top: 20px;
  margin-right: 20px;
  border: 5px solid rgb(255, 189, 67);
  border-radius: 15px;
  text-align: center;
}
.card-img, .card-img-top {
    border-top-left-radius: calc(0.25rem - 1px);
    border-top-right-radius: calc(0.25rem - 1px);
    height: 350px;
}
/* .ans-option-btn {
  width: 100%;
  font-size: 16px;
  color: #ffffff;
  background-color: #252d4a;
  border-radius: 15px;
  display: flex;
  padding: 5px;
  justify-content: flex-start;
  align-items: center;
  border: 5px solid #234668;
  cursor: pointer;
} */
</style>