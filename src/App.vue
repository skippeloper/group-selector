<script setup lang="ts">
import People from "./components/people/People.vue"
import Groups from "./components/groups/Groups.vue"
import Tile from "./components/tiling/Tile.vue"
import { Ref, ref } from "vue";

const people: Ref<string[]> = ref([])
</script>

<template>
  <div class="w-5/5 h-5/5 flex flex-col justify-center items-center gap-3 m-10">
    <h1 class="text-3xl font-bold underline">Group Selector</h1>
    <Tile>
      <People @response="(response: string[]) => { people = response }" />
    </Tile>
    <Tile>
      <Groups name="Group Settings!" :people="people" />
    </Tile>
  </div>
</template>