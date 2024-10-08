<template>
    <div class="form-container">
      <h2>Добавить новую карточку</h2>
      <form @submit.prevent="addCard">
        <div class="form-group">
          <label for="title">Название:</label>
          <input type="text" id="title" v-model="title" required>
        </div>
        <div class="form-group">
          <label for="description">Описание:</label>
          <textarea id="description" v-model="description" required></textarea>
        </div>
        <div class="buttons">
          <button type="submit" class="add-button">Добавить</button>
          <router-link to="/" class="cancel-button">Отмена</router-link>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: "FormPage",
    data() {
      return {
        title: '',
        description: ''
      };
    },
    methods: {
      addCard() {
        const newCard = {
          id: Date.now(),
          title: this.title,
          description: this.description
        };
        const cards = JSON.parse(localStorage.getItem('cards')) || [];
        cards.push(newCard);
        localStorage.setItem('cards', JSON.stringify(cards));
        this.$router.push('/');
      }
    }
  };
  </script>
  
  <style scoped>
  .form-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
  }
  
  input, textarea {
    width: 100%;
    padding: 8px;
    box-sizing: border-box;
  }
  
  .buttons {
    display: flex;
    justify-content: space-between;
  }
  
  .add-button, .cancel-button {
    background-color: #4caf50;
    border: none;
    color: white;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    text-decoration: none;
    display: inline-block;
  }
  
  .cancel-button {
    background-color: #f44336;
  }
  
  .add-button:hover {
    background-color: #45a049;
  }
  
  .cancel-button:hover {
    background-color: #e53935;
  }
  </style>
  