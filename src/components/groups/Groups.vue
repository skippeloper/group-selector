<script setup lang="ts">
import { Ref, ref } from 'vue';

const props = defineProps({
    name: {
        type: String,
        default: "Groups"
    },
    people: {
        type: Array<String>,
        default: []
    }
})

const groupSize: Ref<number> = ref(1);
const groups: Ref<String[][]> = ref([])

function createGroups(people: String[], groupSize: number) {

    if (people.length !== 0 && groupSize >= 1) {
        groups.value = []
        let tempPeople = [...people]

        while (tempPeople.length > groupSize) {
            let group = []

            for (let i = 0; i < groupSize; i++) {
                const randIndex = getRandomInt(0, tempPeople.length)
                const chosenPerson = tempPeople.splice(randIndex, 1)

                group.push(chosenPerson[0])
            }

            groups.value.push(group)
        }

        groups.value.push(tempPeople)
    }
    else {
        window.alert("Please ensure you add people and set group size to 1 or greater!")
    }
}

function getRandomInt(min: number, max: number) {
    const num = Math.random() * (max - min) + min;
    return Math.floor(num)
}

</script>

<template>
    <h1 class="text-2xl font-bold">{{ props.name }}</h1>
    <div class="flex gap-1 items-center">
        <label>Group size: </label>
        <input type="number" min="1" v-model="groupSize" class="transition duration-300" :class="{ 'border-red-600': groupSize < 1 }">
        <button @click="() => createGroups(props.people, groupSize)">Generate Groups</button>
    </div>
    <div>
        <ul>
            <li v-for="(group, index) in groups" :key="index">
                <span>Group {{ index + 1 }}: {{ group }}</span>
            </li>
        </ul>
    </div>
</template>