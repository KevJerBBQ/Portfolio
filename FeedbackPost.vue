<template>
<!-- Tailwind^2.2.4, vue-multiselect^2.1.6 -->
    <div class="editInfoContainer">
        <div class="d-block text-center">
            <h3 class="font-sans text-4xl tracking-wide bg-gray-50 font-bold text-gray-700 mt-2">New Feedback Type</h3>
        </div>
        
        <div>
            <div class="md:grid md:grid-cols-6">
                <div class=" mt-2 md:mt-0 md:col-span-6">

                    <form action="#">
                        <h4 class="font-medium leading-6 text-gray-700">Information</h4>
                        <div class="bg-gray-50 px-4 py-4 space-y-6 sm:p-6">
                            <!-- Title -->
                            <div>
                                <label for="feedback_description" class="text-gray-600 font-semibold text-lg">
                                    Title
                                </label>

                                <div class="items-center mt-1 flex">
                                    <input required id="titleId" v-model="feedback_info.feedback_title" type="text" name="feedback_title"
                                    class="ml-2 pl-2 focus:ring-indigo-600 h-10 focus:border-indigo-400 flex-1 block w-full rounded-lg rounded-r-md sm:text-sm border-gray-500 shadow-md"
                                    placeholder="Write here...">
                                </div>

                                <p class="font-semibold mt-2 text-sm text-gray-500">Please provide details about this feedback.</p>
                            </div>
                            <!-- Description of the feedback -->
                            <div>
                                <label for="feedback_datefrom" class="text-gray-600 font-semibold text-lg">
                                    Category
                                </label>

                                <!-- Here you can include some dropdowns or selections to identify and categorize the feedback -->

                            </div>

                        </div>
                    </form>

                    <form action="#">
                        <h4 class="font-medium leading-6 text-gray-700">Questions</h4>
                        <div class="px-4 py-2 bg-gray-50 text-right md:px-6">
                            <div v-for="(input, index) in feedback_questions" :key="`questionInput-${index}`" class="my-3 align-items-center justify-content-start">

                                <div class="flex flex-row align-items-center justify-content-start">
                                    <label class="text-gray-600 font-semibold text-md mr-2">{{index+1}}. Question:</label>
                                    <input v-model="input.question" :id="'question'+index" placeholder="Type your question..." class="bg-transparent ml-2 border-b-2 border-gray-300 px-2 flex-1 focus:border-indigo-700  focus:placeholder-gray-700" type="text">

                                    <select required :id="'type'+index" v-model="input.question_type" class="ml-2 bg-transparent text-gray-400 hover:text-gray-700 active:text-gray-700">
                                        <optgroup label="Question type">                
                                            <option hidden value="">Select a type..</option>
                                            <option value="text">Text</option>
                                            <option value="number">Number</option>
                                            <option value="dropdown">Dropdown</option>
                                            <option value="date">Date</option>
                                            <option value="time">Time</option>
                                            <option value="duration">Duration</option>
                                            <option value="multiple">Multiple Answers</option>
                                        </optgroup>
                                    </select>

                                    <svg @click="addQuestionField(input, feedback_questions)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="ml-2 cursor-pointer">
                                        <path fill="none" d="M0 0h24v24H0z"/>
                                        <path fill="#5E57E5" d="M11 11V7h2v4h4v2h-4v4h-2v-4H7v-2h4zm1 11C6.477 22 2 17.523 2 12S6.477 2 12 2s10 4.477 10 10-4.477 10-10 10zm0-2a8 8 0 1 0 0-16 8 8 0 0 0 0 16z"/>
                                    </svg>

                                    <svg v-show="feedback_questions.length > 1" @click="removeQuestionField(index, feedback_questions)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="ml-2 cursor-pointer">
                                        <path fill="none" d="M0 0h24v24H0z" />
                                        <path fill="#C54673" d="M12 22C6.477 22 2 17.523 2 12S6.477 2 12 2s10 4.477 10 10-4.477 10-10 10zm0-2a8 8 0 1 0 0-16 8 8 0 0 0 0 16zm0-9.414l2.828-2.829 1.415 1.415L13.414 12l2.829 2.828-1.415 1.415L12 13.414l-2.828 2.829-1.415-1.415L10.586 12 7.757 9.172l1.415-1.415L12 10.586z"/>
                                    </svg>
                                </div>

                                <div class="ml-5 flex flex-row align-items-center justify-content-start">
                                    <div v-if="input.question_type == 'dropdown' || input.question_type == 'multiple'">
                                        <div v-for="(question_answers, inndex) in input.question_answers" :key="`answerInput-${inndex}`" class="flex flex-row my-3 align-items-center justify-content-start">

                                            <label class="text-gray-600 font-semibold text-md mr-2">Answer {{inndex+1}}:</label>
                                            <input :id="index+'answer'+inndex" v-model="question_answers.answer" placeholder="Type your answer..." class="bg-transparent ml-2 border-b-2 border-gray-300 px-2 flex-1 focus:border-indigo-700  focus:placeholder-gray-700" type="text">
                                            
                                            <input v-if="!question_answers.voidpoints" :id="index+'points'+inndex" v-model="question_answers.points" placeholder="Points for this answer" class="bg-transparent ml-2 border-b-2 border-gray-300 px-2 flex-1 focus:border-indigo-700  focus:placeholder-gray-700" type="number">
                                            <label class="items-center ml-1 flex">
                                                <input v-model="question_answers.voidpoints" type="checkbox" :id="index+'voidpoints'+inndex"
                                                class="focus:ring-blue-400 h-4 w-4 rounded">
                                                <span class="ml-2 font-semibold text-lg text-gray-500">No value</span>
                                            </label>

                                            <svg @click="addAnswerField(question_answers, index)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="ml-2 cursor-pointer">
                                                <path fill="none" d="M0 0h24v24H0z"/>
                                                <path fill="#5E57E5" d="M11 11V7h2v4h4v2h-4v4h-2v-4H7v-2h4zm1 11C6.477 22 2 17.523 2 12S6.477 2 12 2s10 4.477 10 10-4.477 10-10 10zm0-2a8 8 0 1 0 0-16 8 8 0 0 0 0 16z"/>
                                            </svg>

                                            <svg v-show="feedback_questions[index].question_answers.length > 1" @click="removeAnswerField(inndex, index)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" class="ml-2 cursor-pointer">
                                                <path fill="none" d="M0 0h24v24H0z" />
                                                <path fill="#C54673" d="M12 22C6.477 22 2 17.523 2 12S6.477 2 12 2s10 4.477 10 10-4.477 10-10 10zm0-2a8 8 0 1 0 0-16 8 8 0 0 0 0 16zm0-9.414l2.828-2.829 1.415 1.415L13.414 12l2.829 2.828-1.415 1.415L12 13.414l-2.828 2.829-1.415-1.415L10.586 12 7.757 9.172l1.415-1.415L12 10.586z"/>
                                            </svg>
                                        
                                        </div>
                                    </div>
                                </div>

                            </div>
                            <div class="row">
                                <p class="font-semibold mt-2 text-sm text-gray-500">Please, list the questions will be part of your your feedback.</p>
                            </div>
                        </div>
                    </form>

                    <div class="px-4 py-2 text-right md:px-6">
                        <button :class="{'cursor-default' : this.sending, 'hover:bg-indigo-700' : !this.sending }" @click="sendfeedback" 
                        class="uppercase inline-flex justify-center py-2 px-4 border border-transparent shadow-md text-sm font-medium rounded-md text-white bg-indigo-600">
                            <span :class="{'hidden' : !this.sending}">Save</span>
                            <span :class="{'hidden' : this.sending}">Sending...</span>
                        </button>
                    </div>

                </div>
            </div>

        </div>
    </div>
</template>

<script>
// Import getters and setters
import Multiselect                          from 'vue-multiselect'
    export default {
        components: { 
            Multiselect
        },
        data(){
            return{
                feedback_info : {},
                feedback_questions: [{ 
                    question: "",
                    question_type : "",
                    question_answers: [{ 
                        answer: "", 
                        points: "", 
                        voidpoints: false 
                    }]
                }],
                questions: 1,
                sending: false,

            }
        },
        computed: {
            // Getters
        },
        props:{
          
        },
        methods:{
            customFormat({Campaign, Location, Title}){
                return `${Campaign} (${Location}) - [${Title}]`;
            },

            addQuestionField(value, feedback_questions) {
                feedback_questions.push({ question: "", question_type: "", question_answers: [{ answer: "", points: "", voidpoints: false }] });
            },

            removeQuestionField(index, feedback_questions) {
                feedback_questions.splice(index, 1);
            },

            addAnswerField(value, index) {
                this.feedback_questions[index].question_answers.push({ answer: "", points: "", voidpoints: false });
            },

            removeAnswerField(inndex, index) {
                this.feedback_questions[index].question_answers.splice(inndex, 1);
            },

            sendfeedback () {
                event.preventDefault();

                // Starts validation inside QUESTIONS
                let key = 0;
                let counter = [];
                [].forEach.call(this.feedback_questions, element => {
                    if(element.question === "" ){
                            document.getElementById('question'+key).classList.add('border-2', 'border-red-500')
                            counter.push({question_label : 'empty'})
                    }else{
                        document.getElementById('question'+key).classList.remove('border-2', 'border-red-500')
                    }
                    
                    if(element.question_type === ""){
                            document.getElementById('type'+key).classList.add('border-2', 'border-red-500')
                            counter.push({question_type : 'empty'})
                    }else{
                        document.getElementById('type'+key).classList.remove('border-2', 'border-red-500')
                    }

                    if(element.question_type === "dropdown" || element.question_type === "multiple"){
                        let answerKey = 0;
                        [].forEach.call(this.feedback_questions[key].question_answers, element => {
                        // For Answers
                            if(element.answer === undefined || element.answer === null || element.answer === ""){
                                    document.getElementById(key+'answer'+answerKey).classList.add('border-2', 'border-red-500')
                                    counter.push({answer : "Q-"+(answerKey+1)+",A-"+(key+1)})
                            }else{
                                document.getElementById(key+'answer'+answerKey).classList.remove('border-2', 'border-red-500')
                            }
                        // For the points (score)
                            if(element.voidpoints == false && (element.points === undefined || element.points === null || element.points === "")){
                                    document.getElementById(key+'points'+answerKey).classList.add('border-2', 'border-red-500')
                                    counter.push({points : "Q-"+(answerKey+1)+",A-"+(key+1)})
                            }else if(element.voidpoints == false){
                                document.getElementById(key+'points'+answerKey).classList.remove('border-2', 'border-red-500')
                            }
                            answerKey++;
                        });
                    }

                    key++;
                });
                // Ends validation inside QUESTIONS

                // Starts validation inside INFORMATION
                if(this.feedback_info.feedback_title === undefined || this.feedback_info.feedback_title === null || this.feedback_info.feedback_title === "" ){
                    document.getElementById('titleId').classList.add('border-2', 'border-red-500')
                    counter.push({feedback_title : "empty"})
                }else{
                    document.getElementById('titleId').classList.remove('border-2', 'border-red-500')
                }

                if(this.feedback_info.feedback_applyto_checkbox){ this.feedback_info.feedback_applyto = this.getCampaignList; }
                if(!this.feedback_info.feedback_applyto_checkbox && (this.feedback_info.feedback_applyto === undefined || this.feedback_info.feedback_applyto === null || this.feedback_info.feedback_applyto === "" || this.feedback_info.feedback_applyto === [])){
                    document.getElementById('applyId').classList.add('border-2', 'border-red-500')
                    counter.push({feedback_applyto : "empty"})
                }else{
                    if(!this.feedback_info.feedback_applyto_checkbox){
                        document.getElementById('applyId').classList.remove('border-2', 'border-red-500')
                    }
                }

                if(this.feedback_info.feedback_applyfrom_checkbox){ this.feedback_info.feedback_applyfrom = this.getCampaignList; }
                if(!this.feedback_info.feedback_applyfrom_checkbox && (this.feedback_info.feedback_applyfrom === undefined || this.feedback_info.feedback_applyfrom === null || this.feedback_info.feedback_applyfrom === "" || this.feedback_info.feedback_applyfrom === [])){
                    document.getElementById('usingId').classList.add('border-2', 'border-red-500')
                    counter.push({feedback_applyfrom : "empty"})
                }else{
                    if(!this.feedback_info.feedback_applyfrom_checkbox){
                        document.getElementById('usingId').classList.remove('border-2', 'border-red-500')
                    }
                }
                // Ends validation inside INFORMATION
                if (counter.length > 0) {
                    this.$fire({
                        type: 'warning',
                        title: "OOPS!",
                        text: "There's some missing information",
                        showConfirmButton: true,
                        confirmButtonColor: '#5E57E5',
                        allowOutsideClick: false,
                        background: '#FFFFFFD9',
                    })
                }else{
                    this.sending = true;
                    let objRequest = {
                        feedback_info:        this.feedback_info,
                        feedback_questions:   this.feedback_questions,
                        feedback_apply:       this.feedback_info.feedback_applyto,
                        feedback_using:       this.feedback_info.feedback_applyfrom
                    }
                    
                    
                    // Here you can add a POST Request to save the information
                    
                }
            }
           
        },
        created() {
            // Call necessary information, in my case of having a tag or category mark for feedbacks
        }
    }
</script>

<style lang="scss" scoped>
    .hide{
        display: none;
    }
</style>
