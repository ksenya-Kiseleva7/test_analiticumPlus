<template>
    <div class="main-container">
      <div class="header">
        <router-link to="/form" class="add-link">
          <button class="add-button">Добавить</button>
        </router-link>
        <label class="switch">
          <input type="checkbox" v-model="isDragEnabled">
          <span class="slider"></span>
        </label>
        <div class="text" style="font-size: 20px;">
          Click&drag
        </div>
        <div class="name" style="font-size: 24px;">Dashboard</div>
      </div>
      <div
        class="card-container"
        @dragover.prevent
        @drop="handleDrop"
      >
        <div
          v-for="card in filteredCards"
          :key="card.id"
          class="card"
          :data-id="card.id"
          :draggable="isDragEnabled"
          @dragstart="isDragEnabled ? handleDragStart(card) : null"
          @dragover="isDragEnabled ? handleDragOver : null"
          @dragend="isDragEnabled ? handleDragEnd : null"
        >
          <div class="card-content">
            <h3>{{ card.title }}</h3>
            <p class="descr">{{ card.description }}</p>
          </div>
          <button class="delete-dashboard" @click="deleteCard(card.id)">Удалить</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "HomePage",
    data() {
      return {
        cards: [],
        draggedCard: null,
        isDragEnabled: true,
      };
    },
    mounted() {
      const localCards = JSON.parse(localStorage.getItem("cards"));
      if (localCards) {
        this.cards = localCards;
      } else {
        this.cards = [
          { id: 1, title: "Пример 1", description: "Описание примера 1" },
          { id: 2, title: "Пример 2", description: "Описание примера 2" },
        ];
        this.saveCardsToLocalStorage();
      }
    },
    computed: {
      filteredCards() {
        return this.cards.filter(card => card !== null);
      }
    },
    methods: {
      handleDragStart(card) {
        this.draggedCard = card;
      },
      handleDragOver(e) {
        e.preventDefault();
      },
      handleDrop(e) {
        if (!this.draggedCard) {
          return;
        }
        const indexToInsert = Array.from(e.target.parentElement.children).indexOf(e.target);
        const draggedIndex = this.cards.indexOf(this.draggedCard);
        this.cards.splice(draggedIndex, 1);
        this.cards.splice(indexToInsert, 0, this.draggedCard);
        this.saveCardsToLocalStorage();
        this.draggedCard = null;
      },
      handleDragEnd() {
        this.saveCardsToLocalStorage();
      },
      saveCardsToLocalStorage() {
        localStorage.setItem("cards", JSON.stringify(this.cards));
      },
      deleteCard(cardId) {
        if (confirm("Вы точно хотите удалить выбранную карточку?")) {
          this.cards = this.cards.filter(card => card.id !== cardId);
          this.saveCardsToLocalStorage();
        }
      }
    }
  };
  </script>
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Playwrite+IT+Moderna:wght@100..400&family=Playwrite+MX:wght@100..400&display=swap');
  .main-container {
    /* width: 1170px; */
    max-width: 1170px;
    margin: 0 auto;
  }
  
  .header {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .add-link {
    text-decoration: none;
    margin-right: 15px;
  }
  
  .name {
    margin-left: auto;
    font-family: "Playwrite IT Moderna", cursive;
    font-optical-sizing: auto;
  }
  
  .add-button {
    background-color: #4caf50;
    border: none;
    color: white;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s;
  }
  
  .add-button:hover {
    background-color: #45a049;
  }
  
  .card-container {
    display: grid;
  
    grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-top: 30px;
}

.card {
  position: relative;
  max-width: 360px; 
  min-height: 120px; 
  background-color: #ffffff;
  padding: 10px 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  cursor: grab;
  transition: box-shadow 0.3s;
}

.card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.card-content {
  margin-bottom: 10px;
}

.card h3 {
  margin: 0 0 10px;
  font-size: 20px;
}

.descr {
  margin: 0;
  margin-bottom: 20px;
}
</style>
