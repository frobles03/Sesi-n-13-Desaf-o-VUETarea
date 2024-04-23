<template>
  <div>
    <nav>
      <ul>
        <li><a href="#" @click="createNewBoard">Añadir una lista</a></li>
      </ul>
      <h3>instrucciones:</h3>
      <h4>Si desea modificar el nombre de una tarea o de la lista de quehaceres, haga clic en ella. Seguido de esto
        aparecerá un evento "Dom" para modificar el texto seleccionado.</h4>
      <h4>para agreagar una tarea a la lista deve escribir en el recuadro blanco y luego precionar la tacla "Enter"
      </h4>
    </nav>

    <div class="boards-container">
      <div class="boards">
        <div class="board" v-for="board in boards" :key="board.id">
          <div class="board-header">
            <span @click="renameBoard(board)">{{ board.name }}</span>
          </div>
          <div class="input">
            <input v-model="text" @keydown.enter="handleNewItem(text, board)" />
          </div>
          <div class="item drag-el" v-for="item in board.items" :key="item.id">
            <div class="task-header">
              <span @click="renameTask(item)">{{ item.title }}</span>
              <button @click="removeTask(board, item)">Eliminar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";

const text = ref("");
const boards = reactive([
  {
    id: "1",
    name: "board-1",
    items: [{ id: "1", title: "Hola a todos" }],
  },
]);

function handleNewItem(text, board) {
  board.items.push({ id: Math.random().toString(36).substr(2, 9), title: text });
}

function createNewBoard() {
  const name = prompt("Name of board");
  if (name) {
    const board = {
      id: Math.random().toString(36).substr(2, 9),
      name: name,
      items: [],
    };
    boards.push(board);
  }
}

function renameBoard(board) {
  const newName = prompt("Enter new name for the board", board.name);
  if (newName && newName !== board.name) {
    board.name = newName;
  }
}

function renameTask(task) {
  const newName = prompt("Enter new name for the task", task.title);
  if (newName && newName !== task.title) {
    task.title = newName;
  }
}

function removeTask(board, task) {
  const index = board.items.indexOf(task);
  if (index !== -1) {
    board.items.splice(index, 1);
  }
}
</script>

<style scoped>
.boards {
  display: flex;
  gap: 10px;
}

.board {
  background: #ccc;
  padding: 15px;
}

.board-header {
  cursor: pointer;
}

.task-header {
  cursor: pointer;
}
</style>
