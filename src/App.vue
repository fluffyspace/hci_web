<script setup>
    import Polja from './components/Polja.vue'
</script>
<style scoped>
.cancel{
  color:red;
  float:right;
  cursor: pointer;
}
.cancel:hover{font-weight: bold;}
</style>
<template>
  <div style="display:flex;flex-wrap: wrap;">
    <div class="m-2">
      <h3>JSON</h3>
      <textarea v-model="json" class="mb-2 form-control" style="width: 300px;"></textarea>
      <button @click="toJson" class="btn btn-success mb-2">Izvezi u JSON</button><br>
      <button @click="importClicked" class="btn btn-danger">Uvezi iz JSON-a</button>
    </div>
    <div class="m-2">
      <h3>Aplikacije</h3>
      <ul class="list-group">
        <li v-for="app in apps" class="list-group-item">{{app}} <span class="cancel" @click="deleteApp(app)" title="Izbriši aplikaciju">X</span></li>
        <li class="list-group-item"><input @keyup.enter="addApp" v-model="newapp" placeholder="Dodaj novu aplikaciju" class="form-control"></li>
      </ul>
    </div>
    <div class="m-2">
      <h3>Zadaci</h3>
      <div>
        <select v-model="odabraniZadatak" class="form-select mb-2">
          <option v-for="zadatak in zadaci" :value="zadatak">{{ zadatak.name }}</option>
          <option :value="null" v-if="zadaci.length == 0" default hidden>Nema zadataka još</option>
          <option :value="null" v-if="zadaci.length != 0" default hidden>Odaberi zadatak</option>
        </select>
      </div>
      <button @click="newZadatak" class="btn btn-primary mb-2">Dodaj zadatak</button><br>
      <button @click="duplicirajZadatak" v-if="odabraniZadatak != null" class="btn btn-primary mb-2">Dupliciraj zadatak</button><br>
      <button @click="deleteZadatak" v-if="odabraniZadatak != null" class="btn btn-danger">Izbriši zadatak</button>
    </div>
    <div class="m-2" v-if="odabraniZadatak != null">
      <Polja :zadatak="odabraniZadatak" :apps="apps"/>
    </div>
  </div>
  <!-- Modal -->
  <div class="modal fade" id="importModal" tabindex="-1" aria-labelledby="importModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="importModalLabel">Uvoženje zadataka iz JSON-a?</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          Trenutni zadaci na web stranici će biti izbrisani i zamijenjeni s zadacima iz JSON teksta.
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Odustani</button>
          <button type="button" class="btn btn-primary" @click="fromJson">Nastavi</button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
    data: function () {
        return {
            zadaci: [],
            odabraniZadatak: null,
            apps: [],
            newapp: null,
            json: null,
            myModal: null,
        };
    },
    props: { myId: String },
    methods: {
      addApp(){
        this.apps.push(this.newapp)
        this.newapp = null
      },
      deleteApp(app){
        for(let zadatak of this.zadaci){
          zadatak.apps = zadatak.apps.filter(it => it != app)
        }
        this.apps = this.apps.filter(it => it != app)
      },
      deleteZadatak(){
        this.zadaci = this.zadaci.filter(it => it != this.odabraniZadatak)
        this.odabraniZadatak = null
      },
      newZadatak(){
        this.zadaci.push({id: 0, apps: [], isExercise: true, name: "Novi zadatak " + this.zadaci.length, approxTime: 0, instructions: "", repetitions: 0})
        this.odabraniZadatak = this.zadaci[this.zadaci.length-1]
      },
      duplicirajZadatak(){
        this.zadaci.push(JSON.parse(JSON.stringify(this.odabraniZadatak)))
        this.odabraniZadatak = this.zadaci[this.zadaci.length-1]
      },
      importClicked(){
        if(this.zadaci.length > 0){
          this.myModal.show()
        } else {
          this.fromJson()
        }
      },
      toJson(){
        this.json = JSON.stringify(this.zadaci)
      },
      fromJson(){
        console.log(this.json)
        this.zadaci = JSON.parse(this.json)
        for(let zadatak of this.zadaci){
          for(let app of zadatak.apps){
            if(!this.apps.includes(app)){
              this.apps.push(app)
            }
          }
        }
      }
    },
    created() {
        
    },
    mounted() {
      this.myModal = new bootstrap.Modal(document.getElementById('importModal'))
    }
};
</script>
