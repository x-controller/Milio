<script setup>
import Store from "electron-store";
import { onMounted, reactive } from "vue";

const store = new Store();

const state = reactive({
  noteList: [],
  form: {
    newNote: {}
  }
});

onMounted(() => {
  getNoteList();
});

const onSaveNote = () => {
  const noteList = state.noteList;
  noteList.push(state.form.newNote);
  store.set("noteList", noteList);
  state.form.newNote = {};
  alert("save success");
};

const getNoteList = () => {
  state.noteList = store.get("noteList");
};
</script>

<template>
  <div>
    <div class="add-note-box">
      <input type="text" v-model="state.form.newNote.content" placeholder="content">
      <button @click="onSaveNote">save</button>
    </div>
    <div class="note-box">
      <div v-for="(note,index) in state.noteList" :key="index">
        {{ note.content }}
      </div>
    </div>
  </div>
</template>

<style lang="less">
</style>
