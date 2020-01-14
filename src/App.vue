<template>

  <div id="app">
   <Header 
       :numCorrect="numCorrect" 
       :numTotal="numTotal" 
   />
    <b-container class="bv-example-row">
    <b-row>
        <b-col sm="6" offset="3" >
					<!-- V-bind  Whatever variable i define here 
					can be used inside the CHILD component and will bind the data i set it equal to,
					refferencing it in HTML via {{}} and in JavaScript via props-->

					<!-- Don't render without data [with empty length array] -->
					<QuestionBox 
						v-if="questions.length" 
						:currentQuestion="questions[index]" 
						:next="next"
            :increment="increment"
					/>
				</b-col>
    </b-row>
    </b-container>
  </div>
  
</template>

<script>
	import QuestionBox from './components/QuestionBox.vue'
	import Header from './components/Header.vue'

	export default {
		name: 'app',
		components: {
			Header,
			QuestionBox,
		}, // 4 holding questions array. Vue DevTools to inspect
		data() {
			return {
				questions:[],
				index: 0, // QBox component should be aware of current question to display one by one
				numCorrect: 0,
				numTotal: 0,
			}
		},
		methods: { 
			//A method to increment my index for Next question
			next(){ //Must be passed to component as well, increment index by 1
				this.index++
			},
			increment(isCorrect){
					if (isCorrect){
						this.numCorrect++
					}
					this.numTotal++
			}
},
		//1 get data on  mount
		mounted: function(){
			fetch('https://opentdb.com/api.php?amount=10&category=9&difficulty=easy&type=multiple', {
				method: "get"
			}) //2 json it
			.then((response) => {
				return response.json()
			}) //3 put it in data.questions array
			.then((jsonData) => {
				this.questions = jsonData.results;
			})		

		}
	}
</script>

<style>
    #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    }
</style>
