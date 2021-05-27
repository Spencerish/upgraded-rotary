<template>
  <div class="conatainer">
        
        <div v-if="!viewingSurveys" class="form-section">
        <h1 class="align-center">
            Survey Form
        </h1>
        <p class="errors" v-if="errors.length">
          Please include all required fields and try again
        </p>
        <div>
          <label for="">First Name:</label>
          <input v-model="programmer.firstName" type="text" placeholder="e.g Eleanor">
        </div>

        <div>
          <label for="">Last Name: </label>
          <input v-model="programmer.lastName" type="text" placeholder="e.g Dorian">
        </div>
        
        <div>
          <label for="">Favorite Programming Language: </label>
          <input type="text" placeholder="e.g Java" v-model="programmer.programmingLanguage">
        </div>

        <button type="submit" @click.prevent="validateFormdData()">
          Submit
        </button>
        <button type="submit" @click.prevent="viewSurveys()">
          View Surveys
        </button>
      </div>
    <admin @closeSurveys="viewingSurveys = false" v-else>
    </admin>
  </div>
</template>

<script>
import Admin from './Admin.vue'

export default {
  name:'app',
  components: {
    Admin
  },  
  data() {
    return {
      programmer:{
        firstName: "",
        lastName: "",
        programmingLanguage: "",
      },
      errors: [],
      viewingSurveys: false
    }
  },
methods: {
  validateFormdData(){
    this.errors = []
    if (!this.programmer.firstName || !this.programmer.lastName || !this.programmer.programmingLanguage) 
     return this.errors.push("Fields cannot be empty")
    this.getFormData() 
  },
  getFormData() {
    if(!localStorage.getItem('survey')) return this.downloadLink(this.cleanObject(this.programmer))
    const survey = localStorage.getItem('survey')
    const newSurvey = this.cleanObject(this.programmer)
    let objArray = []
      objArray.push(survey)
      objArray.push(newSurvey)
    this.downloadLink(objArray) 
  },
  cleanObject(programmerObject){  
    return JSON.stringify(programmerObject) 
  },
  viewSurveys(){
    this.viewingSurveys = true
    console.log('We are viewing now:>> ');
  },
  async downloadLink(surveyText){
    const surveyTextBlob = new Blob([surveyText],  {type: 'text/plain'})
    let textFileLink = document.createElement("a");
    textFileLink.download = "programmer_survey.txt";
    if (window.webkitURL !== null) {
      textFileLink.href = window.webkitURL.createObjectURL(surveyTextBlob);
    }
    else {
      textFileLink.href = window.URL.createObjectURL(surveyTextBlob);
      textFileLink.style.display = "none";
      document.body.appendChild(textFileLink);
    }
    localStorage.setItem('survey', surveyText);
    textFileLink.click();      
  }
},  
}
</script>

<style >
body {
  background-color: #ddddddfb !important;
}
label {
  display: block;
}
input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
input[type=text]:focus {
  background-color: rgba(206, 226, 233, 0.774);
}
input[type=submit] {
  width: 100%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
input[type=submit]:hover {
  background-color: #45a049;
}
button {
  margin-left: 20px;
  margin-right: 20px;
  margin-top: 5px;
  margin-bottom: 5px;
}
button:focus {
  background-color: rgba(169, 219, 238, 0.849);
}
.text-container{
  max-width: fit-content;
}
div {
  border-radius: 5px;
  padding: 20px;
}
.conatainer {
  display: flex;
  align-items: center;
  justify-content: center;
}
.form-section{
  display: flex;
  flex-direction: column;
}
.align-center{
  display: flex;
  justify-content: center;
}
.flexed-between{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.small-btn{
  height: 50%;
  margin: 0 2em 0 2em;
}
</style>