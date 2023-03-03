<template>
     <div class="main">
    <div class="register">
    <div id="visa">
      <h1>Juniper Visa Application</h1>
      <form>
        <label for="first name">First Name:</label>
        <input type="text" v-model="fname" required>
        <br>
        <label for="last name">Last Name:</label>
        <input type="text" v-model="lname" required>
        <br>
        <label for="country">Nationality:</label>
        <input type="text" v-model="nationality" required>
        <br>
        <label for="passport number">Passport Number:</label>
        <input type="text" v-model="passport" required>
  
        <label for="duration">Duration of stay:</label>
        <input type="text" v-model="duration" required>
        <br>
        <label>Select your Gender :</label>
        <select name="gender" id="gender"  v-model="gender" required>
            <option value="" disabled selected>Select your Gender</option>
            <option name="male" >Male</option>
            <option name="female" >Female</option>
            <option name="other" >Other</option>
        </select>
        <br>
        <label>Select your Department :</label>
        <select name="depts" id="department" v-model="department" required>
            <option value="" disabled selected>Select your Department</option>
            <option name="testeng" >Test Engineering</option>
            <option name="rpd" >RPD</option>
            <option name="security" >Security</option>
            <option name="platform" >Platform</option>
      </select>
      <br>
       <br><br>
        <button @click="addVisa" >Add another previous visa</button>
        <br>
        
        <div class="previous" 
        v-for="(applicant, counter) in applicants" 
        v-bind:key="counter">
          <span @click="deleteVisa(counter)" >x</span>
          <label for="duration">{{counter+1}}. Previous Visa:</label>
          <input type="text" v-model="previous" required>
          
        </div>
        <button v-on:click="submit">Submit</button>
      </form>
  </div>
</div>
</div>
  </template>
  
<script>
import axios from 'axios'
export default {
  name: 'App',
  props: {
    msg: String
  },
  data(){
    return{
        fname: '',
        lname: '',
        nationality: '',
        passport: '',
        duration: '',
        gender: '',
        department: '',
        previous:''
        
    }, {
       applicants:[
       {
      previous: '',
      expiration:''
       }
     ]
    }
  },
  methods : {
    addVisa(){
      this.applicants.push({
        previous:'',
        expiration: ''
      })
    },
    deleteVisa(counter)
    {
      this.applicants.splice(counter,1);
    },

    async submit()
            {
                let result = await axios.post(" http://localhost:3000/users",{
                    Fname: this.fname,
                    Lname: this.lname,
                    Nationality: this.nationality,
                    Passport: this.passport,
                    Duration: this.duration,
                    Gender: this.gender,
                    Department: this.department,
                    PreviousVisa: this.previous

                });

                console.warn(result);
                if( result.status==201)
                {
                    alert("Successfully Submitted !");
                    localStorage.setItem("user-info",JSON.stringify(result.data))
                    
                }
            }



  }
}
</script>

<style scoped>
#visa {
  margin: 20px auto;
  max-width: 700px;
}
label{
  display: block;
  margin: 20px 0 10px;
}
input {
  font-size: 30px;
  border: 1px double rgb(102, 97, 96) ;
  border-radius: 4px;
}
button {
  font-size: 18px;
 background: deepskyblue;
 color: #fff;
  padding: 0.4rem 1.3rem;
  text-align: center;
  border: none;
  cursor: pointer;
  border-radius: 4px;
 margin-top: -15px;
 margin-bottom: 10px;
}
span{
  width: 30px;
  float: right;
  cursor: pointer;
}
span:hover{
  color: brown;
}
.previous{
  border: 1.5px solid;
  font-size: 20px;
  padding:5px;
  margin-bottom: 10px;
}
div.register{
    background-color: white;
    padding: 10px;
    padding-right: 130px;
    padding-left: 130px;
    width: 60%;
    height: 230%;
    font-size: 18px;
    border-radius: 10px; 
    border: 1px soid rgba(255,255,255,0.3);
    box-shadow: 2px 2px 15px rgba(0,0,0,0.3);
}
div.main{
    height: 420px;
    width: 700px;
    margin: 0px auto 0px auto;
}
.register select {
    width: 320px;
    height: 40px;
    padding-left: 20px;
    display: block;
    margin-bottom: -25px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid black;
    cursor: pointer;
    border-radius: 4px; 
}

</style>
  