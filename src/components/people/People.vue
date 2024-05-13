<script setup lang="ts">
import { ref, Ref, watch } from 'vue';

const props = defineProps({
    name: {
        type: String,
        default: "People to add!"
    }
})

//Keep track of people
const people: Ref<string[]> = ref([])
const currentPerson: Ref<string> = ref("")

//Event emitter
const emit = defineEmits(["response"])

//Emit people list to parent when list of people changes
watch(people, () => {
    emit("response", people.value)
}, { deep: true })

function addPerson() {
    if (currentPerson.value) {
        people.value.push(currentPerson.value)
        currentPerson.value = ""
    }
}

function removePerson(index: number) {
    people.value.splice(index, 1)
}

</script>

<template>
    <div class="w-1/5 min-w-40">
        <h1 class="text-2xl font-bold">{{ props.name }}</h1>
    </div>
    <div class="flex gap-1 items-center">
        <label>Name: </label>
        <input type="text" placeholder="Type name here" v-model="currentPerson" @keydown.enter="addPerson()">
        <button @click="addPerson()">Add person</button>
    </div>
    <div class="flex flex-row flex-wrap gap-2">
        <div v-if="people.length !== 0" v-for="(person, index) in people" :key="index" :value="person"
            class="bg-slate-600 w-5/12 min-w-40 p-1 px-3 rounded flex flex-row justify-between items-center">
            {{ person }}
            <button @click="removePerson(index)" class="rounded-lg w-8">X</button>
        </div>
        <p v-else>Please add some people.</p>
    </div>
</template>