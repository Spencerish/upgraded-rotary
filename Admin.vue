<template>
  <div class="container ">
      <div class="form-section">
        <h1 class="align-center">
            Surveys
        </h1>
        <div class="text-container">
            {{surveys }}
        </div>

            <button class="small-btn"  type="submit" @click.prevent="back()">
            Back
            </button>
      </div>
  </div>
</template>

<script>
export default {
name: 'admin',
data() {
  return {
    surveys: null
  }
},
methods: {
  back() {
    this.$emit('closeSurveys')        
  },
  getStorage(){
    if(localStorage.getItem('survey')){
      this.surveys = localStorage.getItem('survey')
    }else{
      this.surveys = 'No records' 
    }
  },
 },
 mounted () {
    this.getStorage();
 },
}
</script>

<style>

</style>