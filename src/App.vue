<script setup>
import { ref } from "vue";
import { Icon } from "@iconify/vue";

const showModal = ref(false);
const newNote = ref("");
const errorMsg = ref("");
const notes = ref([]);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (note.value.length < 10) {
    return (errorMsg.value = "Note needs to be 10 characters or more.");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toLocaleDateString("pt-BR"),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
  errorMsg.value = "";
};

const deleteCard = (id) => {
  const index = notes.value.findIndex((note) => note.id === id);
  if (index !== -1) {
    notes.value.splice(index, 1);
  }
};
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
          placeholder="Type your note here..."
        ></textarea>
        <span v-if="errorMsg">{{ errorMsg }}</span>
        <button @click="addNote">Add Note</button>
        <button @click="showModal = false" class="close">x</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div v-if="notes.length === 0">
        <p class="empty">You don't have any notes at the moment.</p>
      </div>
      <div class="cards-container">
        <div
          v-for="note in notes"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <div class="bottomCard">
            <Icon
              class="icon"
              icon="raphael:trash"
              color="#000"
              width="22"
              height="22"
              @click="deleteCard(note.id)"
            />
            <p class="date">{{ note.date }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: auto;
  width: 100vw;
}

.container {
  max-width: 1020px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 50%;
  color: white;
  font-size: 20px;
  transition: all 300ms ease-out;
}

header button:hover {
  background: white;
  border: 1px solid black;
  color: black;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  overflow: auto;
  word-break: break-all;
}

.date {
  font-size: 12px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  overflow: auto;
  max-height: 730px;
}

@media (max-width: 510px) {
  .cards-container {
    justify-content: center;
  }
}

.cards-container::-webkit-scrollbar {
  width: 8px;
}

.cards-container::-webkit-scrollbar-track {
  background-color: transparent;
}

.cards-container::-webkit-scrollbar-thumb {
  background-color: #f5dfbf;
  border-radius: 4px;
}

.cards-container::-webkit-scrollbar-thumb:hover {
  background-color: #f5855d;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: rgb(52, 15, 87);
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
  transition: all 150ms ease-out;
}

.modal button:hover {
  background-color: rgb(87, 26, 145);
}

.modal .close {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: white;
  border-radius: 50%;
  color: rgb(214, 47, 47);
  font-size: 15px;
  border: 1px solid rgb(214, 47, 47);
  transition: all 300ms ease-out;
  align-self: center;
}

.modal .close:hover {
  background-color: rgb(214, 47, 47);
  color: white;
}

.modal span {
  color: rgb(214, 47, 47);
  font-size: 12px;
}

textarea {
  font-size: 14px;
  resize: none;
  padding: 15px;
}

.empty {
  text-align: center;
  font-size: 25px;
  margin-top: 2rem;
}

.bottomCard {
  display: flex;
  flex-direction: row-reverse;
  justify-content: space-between;
}

.bottomCard .icon {
  cursor: pointer;
}
</style>
