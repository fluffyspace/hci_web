<script setup>

</script>
<style scoped>
.instructionshtml{
  width: 50%;
  min-width: 500px;
  border: 1px solid black;
  padding: 10px;
  height: 500px;
}
.inline{
  display:inline;
}
</style>
<template>
    <h3>Zadatak {{ zadatak.name }}</h3>
    <div class="mb-2">
      <label for="id" class="form-label">ID:</label>
      <input type="number" v-model="zadatak.id" id="id" class="form-control">
    </div>
    <div class="mb-2">
      <label for="name" class="form-label">Ime vježbe:</label>
      <input type="text" v-model="zadatak.name" id="name" class="form-control">
    </div>
    <div class="form-check">
      <input class="form-check-input" type="checkbox" value="" id="isExercise" v-model="zadatak.isExercise" v-on="1" v-off="0">
      <label class="form-check-label" for="isExercise">
        Je li ovo vježba
      </label>
    </div>
    <div class="mb-2">
      <label for="approxTime" class="form-label">Prosječno potrebno vrijeme u minutama:</label>
      <input type="number" v-model="zadatak.approxTime" id="approxTime" class="form-control">
    </div>
    <div class="mb-2">
      <label for="apps" class="form-label">Aplikacije s kojima je moguće riješiti zadatak:</label>
      <div v-for="app in apps">
        <input type="checkbox" :value="app" v-model="zadatak.apps" :id="'apps_' + app" class="inline"> <label :for="'apps_' + app">{{ app }}</label><br>
      </div>
      <span v-if="apps == null || apps.length == 0" style="color:red"><br>Da bi ovdje bile prikazane aplikacije, dodaj ih u sekciji 'Aplikacije'.</span>
    </div>
    <div class="mb-2">
      <label for="repetitions" class="form-label">Ponavljanja:</label>
      <input type="number" v-model="zadatak.repetitions" id="repetitions" class="form-control">
    </div>
    <div class="mb-2">
      <label for="html" class="form-label">Instrukcije (HTML):</label>
      <textarea v-model="zadatak.instructions" id="html" class="form-control"></textarea>
    </div>
    <div class="mb-2">
      Kako korisnik vidi instrukcije:<br>
      <iframe :srcdoc="zadatak.instructions" class="instructionshtml"></iframe>
    </div>
    <!--<div v-html="zadatak.instructions" class="instructionshtml"></div>-->
</template>

<script>
export default {
    data: function () {
        return {
            zadaci: [],
        };
    },
    props: { zadatak: Object, apps: Array },
    methods: {
        
    },
    created() {
        
    },
    mounted() {

    }
};
</script>
