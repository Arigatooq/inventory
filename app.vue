<script setup>
import { onMounted } from 'vue';
import qwe from './public/favicon.ico'
import asd from './public/asd.jpg'

const grid = ref(Array.from({ length: 5 }, () => Array(5).fill(null)));
const showContent = ref(false)


const showModal = ref(false);
const modalInfo = ref(
  { 
    row: null,
    col: null,
    content: null 
  }
);

grid.value[0][0] = qwe
grid.value[0][1] = asd


let dragSource = { 
  row: null,
  col: null 
};

const dragStart = (event, rowIndex, colIndex) => {
  dragSource = { row: rowIndex, col: colIndex };
  event.dataTransfer.effectAllowed = 'move';
};


const dragEnter = (event, rowIndex, colIndex) => {
  event.target.classList.add('drag-over');
};

const dragLeave = (event, rowIndex, colIndex) => {
  event.target.classList.remove('drag-over');
};

const drop = (event, rowIndex, colIndex) => {
  event.target.classList.remove('drag-over');
  const targetCell = grid.value[rowIndex][colIndex];
  const sourceCell = grid.value[dragSource.row][dragSource.col];

  if (targetCell === null) {
    grid.value[rowIndex][colIndex] = sourceCell;
    grid.value[dragSource.row][dragSource.col] = null;
  }
};
 const showCell = (rowIndex, colIndex, cell) =>{
  const content = grid.value[rowIndex][colIndex]
  if (content) {

    modalInfo.value = {
      row: rowIndex,
      col: colIndex,
      content: cell
    };
    showModal.value = true;
  }
 }
 const closeModal = () => {
  showModal.value = false;
};
</script>

<template>
  <div class="flex-container">
    <div>
    <div v-for="(row, rowIndex) in grid" :key="rowIndex" class="grid-row">
      <div
        v-for="(cell, colIndex) in row"
        :key="colIndex"
        class="grid-cell"
        @dragover.prevent
        @dragenter="dragEnter($event, rowIndex, colIndex)"
        @dragleave="dragLeave($event, rowIndex, colIndex)"
        @drop="drop($event, rowIndex, colIndex)"
        @click="showCell(rowIndex, colIndex, cell)"
      >
        <img
          v-if="cell"
          :src="cell"
          alt="cell image"
          draggable="true"
          @dragstart="dragStart($event, rowIndex, colIndex)"
        />
      </div>
    </div>
    </div>
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <div class="close" @click="closeModal"></div>
        <div class="divImg"><img v-if="modalInfo.content" :src="modalInfo.content" alt="cell image" class="contentImg"/></div>
        <div class="border"></div>
        <button class="delete">Удалить предмет</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .flex-container{
    display: flex;
    justify-content: center;
    gap: 40px;
  }
  .container{
    max-width: 1080px;
    margin: 0 auto;
  }
  .grid-cell {
  width: 150px;
  height: 150px;
  border: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.grid-row{
  display: flex;
}
.contentImg{
  width: 250px;
  height: 250px;
}
.divImg{
  display: flex;
  justify-content: center;
}
.modal-content{
  position: absolute;
  left: 970px;
  border: 1px solid rgb(48, 47, 47);
  width: 350px;
  height: 758px;
  background-color: gray;
}
.close{
  cursor: pointer;
}
.border{
  margin: 15px 0px;
  border: 1px solid rgb(201, 198, 198);
}
.close {
  width: 1em;
  height: 1em;
  background-color: rgb(216, 216, 216);
  margin-left: auto;
  clip-path: polygon(20% 0%, 0% 20%, 30% 50%, 0% 80%, 20% 100%, 50% 70%, 80% 100%, 100% 80%, 70% 50%, 100% 20%, 80% 0%, 50% 30%);
}
.delete{
  margin-top: 390px;
  margin-left: 50px;
  width: 250px;
  border-radius: 11px;
  border: none;
  height: 40px;
  background: rgb(255, 114, 161);
  color: white;
  font-size: 16px;
  cursor: pointer;
}
.delete:hover{
  opacity: 0.5;
}

</style>
