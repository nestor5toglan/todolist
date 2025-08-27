<template>

  <layout>
    <template v-slot:header>
       
    <!-- Background image -->
    <!--<div
      class="p-5 text-center bg-image"
      style="
        background-image: url('https://img.freepik.com/free-photo/programming-script-text-coding-word_53876-64939.jpg?t=st=1732214224~exp=1732217824~hmac=145296fc1385c268034813fe1b270755edb7b6e8229c446f49053e7ba1e77184&w=740');
        height: 350px; width:100%;background-position: center;padding: 2px; margin: 5px;background-repeat: repeat;
      "
    >
      <div class="mask" style="background-color: rgba(0, 0, 0, 0.6);">
        
      </div>
    </div>
    <!-- Background image -->

      <!--test video debut-->
      <div
  class="p-5 text-center bg-image"
  style="
    background-image: url('https://img.freepik.com/free-photo/programming-script-text-coding-word_53876-64939.jpg?t=st=1732214224~exp=1732217824~hmac=145296fc1385c268034813fe1b270755edb7b6e8229c446f49053e7ba1e77184&w=740');
    height: 350px; width:100%; background-position: center; padding: 2px; margin: 5px; background-repeat: repeat;
  "
>
  <iframe width="560" height="315" 
    src="https://www.youtube.com/embed/ID_DE_TA_VIDEO" 
    title="Video player" frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
    allowfullscreen>
  </iframe>
</div>

      <!--test video fin -->
      

    </template>
  </layout>



    <h1 style="color:red;text-align: center; ">Liste des tâches importantes à faire avec une suivie à chaque fin de tâche :</h1>

    <form action="" @submit.prevent="addTodo" style="margin-left: 25%;">

    <fieldset role="group" style="width: 75%;">
      <input v-model="newTodo" type="text" placeholder="Entrer une tâche à faire">
      <button style="background-color: yellow;font-size: 1rem; color: black;" :disabled="newTodo.length==0" >Ajouter</button>

    </fieldset>
  </form>

    <div v-if="todos.length == 0 " style="align-items: center; text-align: center;">Vous n'avez pas de tâche à faire!</div>

    <div v-else style="width: 100%; justify-items:center; margin-bottom: 4em;">

      <button @click="clearTodos" class="supprtout">
      Supprimer toutes les tâches
    </button>
    
      <label style="text-align: center;">
        <input type="checkbox" v-model=" hideCompleted ">
        Masquer les tâches completées
      </label>
      <ul>
        <li v-for="todo in sortedTodos()" :key="todo.date" :class="{completed:todo.completed}">

          <label for=""><input type="checkbox" v-model="todo.completed">
            {{ todo.title  }} <a @click="deleteTodo(index)" class="delete-icon" title="Supprimer cette tâche">🗑️</a>
          </label>
      
        </li>
      </ul>
    

      
    </div>
    

<layout>
    <template v-slot:footer>
      <footer id="footer">
  &copy; 2024 - Mon Application Vue.js | Tous droits réservés | By nextech
</footer>

    </template>
  </layout>

</template>



    <script setup lang="ts">

    import { ref, watch } from 'vue' ;

    import layout from "./layout.vue";



    

 




    //creons un tableau qui stockera le données de la liste  to do
    const todos = ref(JSON.parse(localStorage.getItem('todos')) || [{
      title : 'Tâche de teste',
      completed : true,
      date :  1,
    },{
      title: 'tâche 2',
      completed: false,
      date : 2,
    }] );

    const newTodo = ref('');

    const hideCompleted = ref(false);


    const addTodo  = () =>{
    todos.value.push({
        title: newTodo.value,
        completed: false,
        date : Date.now()
      })

      newTodo.value = '';
      saveTodos(); // Sauvegarder dans localStorage
      }

      //supprimer une tâche spécifique

      const deleteTodo = (index) => {
        todos.value.splice(index,1);
        saveTodos();//sauvegarde dans le localstorage
      }

      //supprimer toutes les tâches 
      const  clearTodos = () => {
        todos.value = [];
        saveTodos();//pour mettre a jour la local storage 
      }

    

    const sortedTodos = () =>{
    const sortedTodos = todos.value.toSorted((a,b) => a.completed > b.completed ? 1 : -1 );
     if(hideCompleted.value == true){
      return  sortedTodos.filter(t => t.completed==false)
     }
     return sortedTodos

    }

    //sauvegarder les tâches dans le localStorage

    const saveTodos = () => {
      localStorage.setItem('todos', JSON.stringify(todos.value));
    }

    //sauvegarder automatiquement  lorsque la liste des tâches changes 

    watch(todos, saveTodos, { deep : true });
         
       

      
    </script>

  <style>

  .completed{
    opacity: 70%;
    text-decoration: line-through;
  }
  

  #footer{
    align-items: center;
    position: fixed; /* Reste fixe en bas de la page */
    bottom: 0; /* Aligné en bas de la fenêtre */
    left: 0; /* Étend sur la largeur complète */
    right: 0;
    width: 100%; /* Pleine largeur */
    height: 5%; /* Hauteur fixe du footer */
    /*background-image: url('https://img.freepik.com/free-photo/programming-script-text-coding-word_53876-64939.jpg');
    background-size: cover; /* Couvre toute la largeur et hauteur du footer //
    background-repeat: no-repeat; /* Empêche la répétition de l'image /
    background-position: center; / Centre l'image */
    color: rgb(255, 255, 255); /* Texte en blanc */
    text-align: center; /* Centre le texte horizontalement */
    padding: 7px; /* Espacement interne pour le contenu */
    font-size: 15px; /* Taille de police réduite */
    background-color: black;
    margin-top: 2em;
    
  }


.delete-icon{
  margin-left: 2em;
  background-color: red;
  padding: 5px;
  border-radius: 50%;
}

.delete-icon:hover{
  background-color: rgb(0, 0, 0);
}

.supprtout:hover{
  background-color: rgb(0, 0, 0);

}

.supprtout{
  margin-top: 20px; 
  margin-bottom: 2em;
  background-color: red; 
  color: white;
}






  </style>
