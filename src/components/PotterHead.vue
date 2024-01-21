<script setup lang="ts">
import { ref } from 'vue';

const url = ref('https://api.potterdb.com/v1/spells/')

const data = ref(null);
const error = ref(null);
const imageUrl = ref('');
const effect = ref('');
const wikiUrl = ref('');
const spellName = ref('');
const incantation = ref('');

const getSpells = async () => {

    return new Promise(function(resolve, reject){
        fetch(url.value)
        .then((response: any) => {
            resolve(response.json());
        })
        .catch((err) => {
            reject(err);
        })
    })
}

const randomSpell = async () => {
    
    const spells: any = getSpells();

    spells.then(function(data: any){
        const random = Math.floor(Math.random() * data.data.length);
        setSpell(data.data[random].attributes);
    }).catch(function(err: any){
        console.log(err);
    })
}

const setSpell = (spell: any) => {

    if(!spell.image){
        randomSpell();
    }
    
    console.log(spell);
    imageUrl.value = spell.image;
    effect.value = spell.effect;
    wikiUrl.value = spell.wikiUrl;
    spellName.value = spell.name;
    incantation.value = spell.incantation;
}

randomSpell();

</script>

<template>
    <div class="container">
            <h3 class="welcome">Welcome, Potterhead</h3>
            <p>Be the master of your own spell</p>

        <div v-if="imageUrl" class="main-section">
            <img :src="imageUrl">

            <p class="spell-name">{{ spellName }}</p>

            <h3>What does this do?</h3>
            <span>{{ effect }} <a :href="wikiUrl">Read more</a></span>
            <p v-if="incantation">Incantation: {{ incantation }}</p>
        </div>
    </div>
</template>

<style>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.header {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.main-section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 2rem;
}

.more-detail {
    margin-top: 2rem;
}

img {
    border: 1px solid #ddd;
    background-color: white;
    border-radius: 4px;
    padding: 5px;
    width: 200px;
    height: 300px;
}

img:hover {
    box-shadow: 0 0 2px 1px rgba(0, 140, 186, 0.5);
}
a {
    text-decoration: none;
}
</style>
