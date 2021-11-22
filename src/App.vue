<template>
  <div id="app">
    <Header username="Sebastian Moser"
            :errormessage="errormessage"/>

    <stundenplan v-if="loginState" :timetableParams="params"
                 @removeLesson="removeLesson"
                 @lessonMoveUp="lessonMoveUp"
                 @lessonMoveDown="lessonMoveDown"
                 @addUnit="addUnit"/>
    <Login v-else/>
  </div>
</template>

<script>
  import stundenplan from './components/Stundenplan.vue'
  import Header from './components/Header.vue'
  import Login from "@/components/Login";
  import {ref} from 'vue'

  export default {
    name: 'App',
    components: {
      stundenplan,
      Header,
      Login
    },

    setup() {

      let errormessage = ref("");
      let loginState =ref(false);
      let params = ref({
                schoolClass: "5BHIT",
                datum: "18.10.2021",
                timeTable: [
                  {beginn:'7:50', end:'8:40', fach:'SYT'},
                  {beginn:'8:45', end:'9:35', fach:'ITP'},
                  {beginn:'9:45', end:'10:35', fach:'Englisch'},
                  {beginn:'10:45', end:'11:40', fach:'Deutsch'},
                  {beginn:'11:40', end:'12:30', fach:'Religion'},
                ],
              }
      )

      function removeLesson(index) {
        console.log("Remove item with index: "+index);
        params.value.timeTable.splice(index,1);
      }

      function lessonMoveUp(index) {
        if(index == 0) return

        console.log("Move item with index up: "+index);
        let tmp = params.value.timeTable[index];
        params.value.timeTable[index] = params.value.timeTable[index - 1];
        params.value.timeTable[index - 1] = tmp;
      }

      //function loginDone() {
      // console.log("App::Login Done!");
      // loginState.value=true;
      // }
      function lessonMoveDown(index) {
        if(index == params.value.timeTable.length - 1) return

        console.log("Move item with index down: "+index);
        let tmp = params.value.timeTable[index];
        params.value.timeTable[index] = params.value.timeTable[index + 1];
        params.value.timeTable[index + 1] = tmp;
      }

      function addUnit(newE) {
        console.log("Parent addUnit: "+ newE)

        //TODO: Alle Felder sollen ausgefüllt sein, damit man ein neues Entry anlegen können

        let newEntry = {
          beginn: newE.beginn,
          end: newE.end,
          fach: newE.fach
        }

        //Überprüft ob eines der Felder leer ist
        if(newE.beginn != "" && newE.end != "" && newE.fach != "" && isTime(newE.beginn, "Beginn") && isTime(newE.end, "End")) {
          params.value.timeTable.push(newEntry);
          errormessage.value = "";
          newE.beginn=""
          newE.end=""
          newE.fach=""
        }
      }


      //00:00
      function isTime(field, fieldname) {

        let ti=field.split(':')
        let number = parseInt(ti[0], 10);

        if(isNaN(number)) {
          errormessage.value="Dies ist kein gültiger Wert (Feld: "+ fieldname + ")"
          return false;
        }
        else {
          if (number < 7 || number > 17) {
            errormessage.value="Dies ist kein gültiger Wert (Feld: "+ fieldname + ")"
            return false;

          }
        }
        return true;
      }


      return {
        params,
        removeLesson,
        lessonMoveUp,
        lessonMoveDown,
        addUnit,
        errormessage,
        loginState
      }
    }
  }
</script>

<style scoped>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
  }
</style>