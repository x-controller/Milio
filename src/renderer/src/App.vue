<script setup>
import Store from "electron-store";
import { onMounted, reactive } from "vue";
import helper from "./helper";

const store = new Store();

const state = reactive({
  noteList: [],
  filter: { keywords: "" },
  form: {
    newNote: {}
  }
});

onMounted(() => {
  getNoteList();
});

const onSaveNote = () => {
  const noteList = state.noteList;
  const newNote = state.form.newNote;
  newNote["id"] = helper.generateRandomString(16);
  noteList.push(newNote);
  store.set("noteList", noteList);
  state.form.newNote = {};
  alert("save success");
};

const getNoteList = () => {
  state.noteList = store.get("noteList");
};

const noteRemove = (id) => {
  const noteList = state.noteList;
  noteList.filter(item => item["id"] === id);

  store.set("noteList", noteList);
  state.noteList = noteList;
  alert("delete success");
};

const onNoteSearch = () => {
  const noteList = state.noteList;
  const filter = state.filter.keywords.toLowerCase();
  state.noteList = noteList.filter(item => item["content"].toLowerCase().includes(filter));
};
</script>

<template>
  <div>
    <div>
      <input type="text" placeholder="filter note">
      <button @click="onNoteSearch">search</button>
    </div>

    <div class="add-note-box">
      <input type="text" v-model="state.form.newNote.content" placeholder="content">
      <button @click="onSaveNote">save</button>
    </div>
    <div class="note-box">
      <div v-for="(note,index) in state.noteList" :key="index">

        <div>{{ note.content }}</div>
        <div>
          <button @click="noteRemove(note['id'])">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="less">
</style>
