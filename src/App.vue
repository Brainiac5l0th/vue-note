<script setup>
  import { ref } from "vue";

  const showModal = ref(false);
  const noteText = ref("");
  const errorMessage = ref("");
  const notes = ref([]);

  //max id generator
  const maxId = (givenArray) => {
    const next = givenArray.reduce(
      (maxId, element) => Math.max(element.id, maxId),0);
    return next + 1;
  };
  //dark color generator
  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 20%)";
  }
  //add new note details to notes array
  const addNote =()=>{
    
    if(noteText.value.length<15){
      return errorMessage.value = "Title of the note should have 15 or more characters!";
    }
    notes.value.push({
      id: maxId(notes.value),
      text: noteText.value,
      date: new Date(),
      backgroundColor: getRandomColor()
    });
    errorMessage.value = "";
    noteText.value = "";
    showModal.value = false;
  }
</script>

<template>
  <main>
    <div class="notes__overlay" v-show="showModal">
      <div class="notes-overlay__modal">
        <textarea v-model.trim="noteText" name="note" id="note" cols="30" rows="15"  placeholder="Type your note title here"></textarea>
        <p v-if="errorMessage" class="notes-overlay__modal-error">{{ errorMessage }}</p>
        <div class="notes-overlay__buttons">
          <button class="notes-overlay__button notes-overlay__button-add" @click="addNote">Create Note</button>
          <button class="notes-overlay__button notes-overlay__button-close" @click="showModal=!showModal">close</button>
      </div>

      </div>
    </div>
    <div class="notes__container">
      <header class="notes-container__header">
        <h1 class="notes-container__header-logo">Notes</h1>
        <button class="notes-container__header-btn" @click="showModal=!showModal">+</button>
      </header>
      <div class="notes-container__card-container">
        <div 
            v-for="note in notes" 
            :key="note.id"
            class="notes-container__card" 
            :style="{ backgroundColor: note.backgroundColor }"
          >
          <p class="notes-card__main-title">{{ note.text }}</p>
          <p class="notes-card__date">{{ note.date.toLocaleString() }}</p>
        </div>
        
      </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width: 100vw;
}
.notes__container{
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
.notes-container__header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255,255,255,0.2);
  border-radius: 10px;
  padding: 10px 30px;
  background: linear-gradient(to right, #00467F, #A5CC82);
}

.notes-container__header-logo{
  font-size: 44px;
  color: white;
  font-weight: 600;
}
.notes-container__header-btn{
  height: 46px;
  width: 46px;
  border-radius: 100%;
  cursor: pointer;
  font-size: 18px;
  border: none;
  outline: none;
  transition: 250ms all linear;
}
.notes-container__header-btn:hover{
  transform: scale(1.1);
}
.notes-container__header-btn:active{
  transform: translateY(3px);
}

.notes-container__card-container{
  width: 100%;
  margin-top: 30px;
  display: flex;
  /* justify-content: center; */
  /* align-items: center; */
  gap: 20px;
  flex-wrap: wrap;
}

.notes-container__card{
  padding: 20px;
  background: teal;
  border-radius: 10px;
  border: none;
  outline: none;
  width: 200px;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.notes-card__main-title{
  color: #fff;
  font-size: 18px;
  font-weight: 500;
}
.notes-card__date{
  font-size: 12px;
}

/* modal  */
.notes__overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(25,255,255, 0.3);
  z-index: 99;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.notes-overlay__modal{
  width: 768px;
  background: var(--color-background);
  border-radius: 10px;
  padding: 32px;
  position: relative;
  display: flex;
  gap: 10px;
  flex-direction: column;
}
.notes-overlay__modal textarea{
  border-radius: 10px;
  padding: 20px;
  resize: none;
  font-size: 16px;
  background: var(--color-background);
  color: #fff;
  border-width: 2pt ;
  /* outline: none; */
}

.notes-overlay__modal textarea:active,
.notes-overlay__modal textarea:focus{
  background: 
              linear-gradient(var(--color-background), var(--color-background)) padding-box,
              linear-gradient(25deg,  #A5CC82, #00467F) border-box;
  border-radius: 10px;
  border: 2pt solid transparent;
  outline: none;
}
.notes-overlay__modal-error{
  color: tomato;
  font-size: 12px;
}
.notes-overlay__buttons{
  display: flex;
  flex-direction: row;
  gap: 10px;
}
.notes-overlay__button{
  padding: 8px 20px;
  border-radius: 10px;
  border: none;
  outline:none;
  color: white;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: 250ms all linear;
}
.notes-overlay__button:hover{
  transform: scale(1.01);
  transform: translateY(-1px);
}
.notes-overlay__button:active{
  transform: translateY(1px);
}
.notes-overlay__button-add{
  flex: 1;
  background: linear-gradient(to right, #23669edb, #87b162);
}
.notes-overlay__button-close{
  background: linear-gradient(to right, tomato, red);
}
</style>