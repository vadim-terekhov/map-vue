<template>
    <div id="app">
        <div class="office">
            <Map 
                @click="handlerClick"
            />
            <SideMenu 
                :person="person"
                :isUserOpenned="isUserOpenned"
                @update:isUserOpenned="handlerUserOpenned"
            />
        </div>
    </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import persons from '@/assets/data/people.json';

export default { 
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data(){
      return {
          persons,
          person: null,
          isUserOpenned: false,
      }
  },
  methods:{
    addContur(el){
        el.classList.add('selected');
    },
    removeContur(){
        const all = document.querySelectorAll('.employer-place');
        all.forEach( i => i.classList.remove('selected'));
    },
    handlerUserOpenned(flag){
        this.isUserOpenned = flag;
        this.removeContur();
    },
    handlerClick(e){
        this.removeContur();
        const idPeople = e.target.closest('.employer-place')?.id;
        if (idPeople !== undefined){
            this.addContur(e.target.closest('.employer-place'));
            const person = this.persons.find( i => i._id === Number(idPeople));
            if (person){
                this.person = person;
            }else{
                this.person = null;
            }
        this.isUserOpenned = true;
        }else{
            this.isUserOpenned = false;
        }
    }
  }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
    background-color: #fafafa;
    padding: 24px;
    box-sizing: border-box;
}

html,
body,
#app {
    height: 100%;
}

* {
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

.office {
    display: grid;
    grid-template-columns: 1fr 320px;
    border-radius: 6px;
    border: 1px solid #ccd8e4;
    height: 100%;
    background: white;
    max-width: 1500px;
    margin: 0 auto;
}
.selected{
    stroke:crimson;
}
</style>
