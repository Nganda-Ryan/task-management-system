<script setup>
  import { ref } from "vue";
  import Task from '@/components/Task.vue'
  import Modal from '@/components/Modal.vue'

  const taskList = ref([]);
  const currentTask = ref({
    id: '',
    title: '',
    description: '',
    todos: [], //[""] 0
    image: null,
    color: ''
  });
  const selectedTask = ref({
    id: '',
    title: '',
    description: '',
    todos: [],
    image: null,
    color: ''
  });
  const isModalOpen = ref(true);
  const taskColors = ["#ffd3b6", "#ececec", "#e7eaf6", "#e0ffcd", "#eaf6f6", "#f0ece2", "#feffdf", "#f5f9ee", "#fcf4d9", "#f4eeff", "#f0fbff", "#fffafa", "#d4f8e8" ]
  const generateCode = (title) => {
    let result = '';

    for (let i = 0; i < 8; i++) {
        const randomIndex = Math.floor(Math.random() * title.length);
        result += title[randomIndex];
    }

    return result;
  }

  // Ajouter un Todo
  const addTodo = () => {
    currentTask.value.todos.push("");
  }

  // Supprimer un Todo
  const removeTodo = (index) => {
    currentTask.value.todos.splice(index, 1);
  }

  // Gestion de l'image
  const handleImageUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      currentTask.value.image = file;
    }
  }

  // Soumission du formulaire
  const submitTask = () => {
    console.log("Tâche enregistrée :", currentTask.value);
    currentTask.value.todos = currentTask.value.todos.filter(t => t != '')
    
    taskList.value.push({
      ... currentTask.value,
      color: taskColors[Math.floor(Math.random() * taskColors.length)],
      id: generateCode(currentTask.value.title)
    });

    currentTask.value = {
      id: '',
      title: '',
      description: '',
      todos: [],
      image: null,
      color: ''
    }
  }

  const handleDelete = (payload) => {
    console.log('item to keep:', taskList.value.filter(task => task.id != payload))
    taskList.value = taskList.value.filter(task => task.id != payload);
    console.log('delete', payload);
  }

  const handleSelect = (payload) => {
    selectedTask.value = taskList.value.find(task => task.id == payload)
    isModalOpen.value = !isModalOpen.value;
  }

  const closeModal = (payload) => {
    isModalOpen.value = !isModalOpen.value;
  }
</script>

<template>
  <div class="container">
    <aside>
      <div class="currentTask-form-container">
        <h1>Enregistrer une Tâche</h1>
        <form @submit.prevent="submitTask">
          <div class="form-group">
            <label for="title">Title</label>
            <input
              type="text"
              id="title"
              v-model="currentTask.title"
              placeholder="Entrez le titre"
              required
            />
          </div>

          <div class="form-group">
            <label for="description">Description</label>
            <textarea
              id="description"
              v-model="currentTask.description"
              placeholder="Entrez la description"
              required
            ></textarea>
          </div>

          <div class="form-group">
            <label for="image">Image</label>
            <input
              type="file"
              id="image"
              @change="handleImageUpload"
              accept="image/*"
            />
          </div>

          <div class="form-group">
            <label>Todos</label>
            <div class="todo-list">
              <div v-for="(todo, index) in currentTask.todos" :key="index" class="todo-item">
                <input
                  type="text"
                  v-model="currentTask.todos[index]"
                  placeholder="Entrez un todo"
                />
                <button type="button" @click="removeTodo(index)">Supprimer</button>
              </div>
            </div>
            <button type="button" @click="addTodo">Ajouter un Todo</button>
          </div>

          <button type="submit" class="submit-button">Enregistrer la tâche</button>
        </form>
      </div>
    </aside>
    
    <main>
      <template v-if="taskList.length > 0">
        <template v-for="task in taskList" :key="task.id" >
          <Task :payload="task" @select="handleSelect" @delete="handleDelete"/>
        </template>
      </template>
      <template v-else>
        <img src="/images/free-panda2.webp" alt="" class="h-96">
      </template>
    </main>
    <template v-if="isModalOpen">
      <Modal :title="currentTask.title" :width="'xs'" @close="closeModal">
        <template #body>
        
        </template>
      </Modal>
    </template>
  </div>
</template>

<style scoped>
  .container {
    display: flex;
    justify-content: space-between;
  }

  aside {
    width: 28%;
    padding: 25px;
    background-color: white;
    box-shadow:
      0 1px 1px hsl(0deg 0% 0% / 0.075),
      0 2px 2px hsl(0deg 0% 0% / 0.075),
      0 4px 4px hsl(0deg 0% 0% / 0.075),
      0 8px 8px hsl(0deg 0% 0% / 0.075),
      0 16px 16px hsl(0deg 0% 0% / 0.075)
    ;
    border: 1px solid #ddd;
    border-radius: 8px;
  }

  main {
    display: flex;
    background-color: white;
    justify-content: space-evenly;
    flex-wrap: wrap;
    width: 65%;
    margin: 0 auto;
    padding: 25px 10px 25px 10px;
    background-color: white;
    box-shadow:
      0 1px 1px hsl(0deg 0% 0% / 0.075),
      0 2px 2px hsl(0deg 0% 0% / 0.075),
      0 4px 4px hsl(0deg 0% 0% / 0.075),
      0 8px 8px hsl(0deg 0% 0% / 0.075),
      0 16px 16px hsl(0deg 0% 0% / 0.075)
    ;
    border: 1px solid #ddd;
    border-radius: 8px;
  }


  h1 {
    text-align: center;
    margin-bottom: 20px;
  }

  .form-group {
    margin-bottom: 15px;
  }

  label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
  }

  input,
  textarea {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  textarea {
    resize: vertical;
  }

  .todo-list {
    margin-top: 10px;
  }

  .todo-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  .todo-item input {
    flex-grow: 1;
    margin-right: 10px;
  }

  button {
    padding: 8px 12px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background-color: #0056b3;
  }

  .submit-button {
    width: 100%;
    background-color: #28a745;
  }

  .submit-button:hover {
    background-color: #218838;
  }

  

  @media only screen and (max-width: 1024px) {
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    aside {
      width: 60%;
    }

    main {
      min-height: 100px;
      width: 80%;
    }
  }

  @media only screen and (max-width: 640px) {
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    aside {
      width: 100%;
      margin-bottom: 40px;
    }

    main {
      width: 100%;
    }
  }

    
</style>
