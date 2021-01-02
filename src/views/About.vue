<template>

  <div class="pure-g">
    <div class="pure-u-2-24">
    </div>
    
    <div class="pure-u-20-24">
    <div class="container">
      <div class="text-box">
      <h1>About Baby Yoda Quiz</h1>
      <h1>About Baby Yoda Quiz</h1>
      </div>
      <p id="one" onmouseover="style.color='gold'" onmouseout="style.color='#484848'">Meet the Team</p>
      </div>
      
      <table class="pure-table">
        <thead>
            <tr>
                <th>Name</th>
                <th>Github</th>
                <th>Student Number</th>
                <th>Class Day</th>
            </tr>
        </thead>
        <tbody>
          <tr v-for="person in team.students" :key="person.name">
            <td>{{person.name}}</td>
            <td> <a :href="person.github">{{person.github}}</a></td>
            <td>{{person.stuNumber}}</td>
            <td>{{person.classDay}}</td>
          </tr>
        </tbody>
      </table>

      <div id="intro">
    
        <input id="btn" @click="showDetails = !showDetails" class="pure-button pure-button-primary" value="Detailed Description">
       
        <transition  name="bounce">
          <p v-if="showDetails">This project is a quiz platform that can be used to create quizzes, 
                        take quizzes, and see your quiz result from a database. We will do 
                        our best to make the web application modular so that the quizzes can
                        be changed easily. We will be able to use forms to add quizzes and 
                        quiz questions to the database. Main idea: Quiz management platform.
                        Save quizzes to local storage.Simpler idea: One static quiz with ability
                        to create user profile and keep track of scores in a database
          </p>
        </transition >
        </div>

    </div>

    <div class="pure-u-2-24">
    </div>
</div>


</template>


 
<script>
export default {
  name: 'About',
  data() {
    return {
      team: {},
      showDetails:true
    }
  },
  // mounted() hook is called when Vue app is mounted
  mounted() {
    this.team = {
        "students": [
          {"name": "Diana", "github": "https://github.com/DianaAvanesy","stuNumber":200421248,classDay:"Tuesday"},
          {"name": "Delroy", "github": "https://github.com/delroydennie","stuNumber":200329242,classDay:"Tuesday"},
          {"name": "Liam", "github": "https://github.com/Rokket14","stuNumber":200427653,classDay:"Tuesday"},
          {"name": "Arezou", "github": "https://github.com/arezousohrabvand","stuNumber":200387480,classDay:"Monday-Section3"},
        ]
      };
  }
}


</script>

<style>
.bounce-enter-active{
  animation:bounce-in .6;
}
.bounce-leave-active{
  animation:bounce-in .6 reverse;
}
@keyframes bounce-in{
  0%{
    transform:scale(0);
  }
  50%{
    transform:scale(1.5);
  }
  100%{
    transform:scale(1);
  }

}

#intro{
  background-color:#f2f2f2;
  position:fixed;
  margin-right:500px;
  padding:20px;
  margin-top:40px;
  text-align:justify;
  border:solid #484848 5px;
}
#btn{
  background-color:#484848;
  padding:10px  20px;
  text-align:center;
  outline:none;
  cursor:pointer;
  border:none;
  border-radius:13px;
  box-shadow:0 7px #b3b3b3;
}
#btn:hover{
  background-color:#484848;
}
#btn:active{
  background-color:#00334d;
  box-shadow:0 7px #e6e6e6;
  transform:translateY(5px);
}
.pure-table{
  border:2px solid #484848;
}
thead td{
  background-color:#E1E7EE;
}
th,
td{
  border-bottom:1px solid #484848;
  line-height:1.2;
  text-align:left;
  padding:0.4rem;
}
th{
  background:#484848;
  font-size:0.5rem;
  color:white;
  text-tranform:uppercase;
}

*,
*:after,
*:before{
  box-sizing:border-box;
}
body{
  font-family:"Inter",sans-serif;
  background-color:#fff;
}

.text-box{
  display:flex;
  align-items:left;
  justify-content:left;
  position:relative;
}

h1{
  font-size:4vw;
  font-weight:600;
  background-color:#484848;
  display:block;
  color:#fff;
  padding:0.5rem;
  text-align:left;
   margin-left:15px;
}
h1:nth-child(2)
{
  background-color:#fff;
  color:#484848;
  position:absolute;
  clip-path:inset(-1% -1% 50% -1%);
}

#one{
  font-size:2vm;
  font-weight:600;
  text-align:left;
  margin-left:260px;
  margin-top:0.1rem;
  color:#484848;
  
}




</style>