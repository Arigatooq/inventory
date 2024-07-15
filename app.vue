<script setup>
import { ref, onMounted, watch } from 'vue';

import Cell_1 from '~/public/Img1.png';
import Cell_2 from '~/public/Img2.png';
import Cell_3 from '~/public/Img3.png';

const grid = ref(Array.from({ length: 5 }, () => Array(5).fill(null)))
const showModal = ref(false)
const modalInfo = ref({
  row: null,
  col: null,
  content: null,
});

const theme = ref('dark')

const initializeGrid = () => {
  const savedGrid = localStorage.getItem('grid')
  if (savedGrid) {
    grid.value = JSON.parse(savedGrid)
  } else {
    grid.value[0][0] = Cell_3;
    grid.value[0][1] = Cell_1;
    grid.value[0][2] = Cell_2;
  }
};

const saveGrid = () => {
  localStorage.setItem('grid', JSON.stringify(grid.value))
}

const saveTheme = () => {
  localStorage.setItem('theme', theme.value)
}

const initializeTheme = () => {
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme) {
    theme.value = savedTheme;
  }
}

onMounted(() => {
  initializeGrid()
  initializeTheme()
})

watch(grid, saveGrid, { deep: true });
watch(theme, saveTheme)

let dragSource = {
  row: null,
  col: null,
}

const dragStart = (event, rowIndex, colIndex) => {
  dragSource = {
    row: rowIndex,
    col: colIndex,
  };
  event.dataTransfer.effectAllowed = 'move';
}

const dragEnter = (event) => {
  event.target.classList.add('drag-over')
}

const dragLeave = (event) => {
  event.target.classList.remove('drag-over')
}

const drop = (event, rowIndex, colIndex) => {
  event.target.classList.remove('drag-over')
  const targetCell = grid.value[rowIndex][colIndex]
  const sourceCell = grid.value[dragSource.row][dragSource.col]

  if (targetCell === null) {
    grid.value[rowIndex][colIndex] = sourceCell
    grid.value[dragSource.row][dragSource.col] = null
  }
}

const showCell = (rowIndex, colIndex, cell) => {
  const content = grid.value[rowIndex][colIndex]
  if (content) {
    modalInfo.value = {
      row: rowIndex,
      col: colIndex,
      content: cell,
    };
    showModal.value = true
  }
};

const closeModal = () => {
  showModal.value = false
};

const getCellClass = (rowIndex, colIndex) => {
  const classes = [];
  if (rowIndex === 0 && colIndex === 0) classes.push('top-left');
  if (rowIndex === 0 && colIndex === 4) classes.push('top-right');
  if (rowIndex === 4 && colIndex === 0) classes.push('bottom-left');
  if (rowIndex === 4 && colIndex === 4) classes.push('bottom-right');
  return classes.join(' ');
};

const deleteItem = () => {
  if (modalInfo.value.row !== null && modalInfo.value.col !== null) {
    grid.value[modalInfo.value.row][modalInfo.value.col] = null
    saveGrid();
    closeModal()
  }
};

const toggleTheme = () => {
  theme.value = theme.value === 'dark' ? 'light' : 'dark';
};
</script>

<template>
  <div :class="`main ${theme}`">
    
  <div class="flex-container">
    <button @click="toggleTheme" class="theme-toggle">
        {{ theme === 'dark' ? 'Light' : 'Dark' }} Theme
      </button>
    <div>
    <div v-for="(row, rowIndex) in grid" :key="rowIndex" class="grid-row">
      <div
        v-for="(cell, colIndex) in row"
        :key="colIndex"
        class="grid-cell"
        :class="getCellClass(rowIndex, colIndex)"
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
      <InfiniteSkeletonBottom/>
  </div>
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <div class="close" @click="closeModal"></div>
        <div class="divImg"><img v-if="modalInfo.content" :src="modalInfo.content" alt="cell image" class="contentImg"/></div>
        <div class="border"></div>
        <InfiniteSkeleton/>
        <div class="btnFlex"><button class="delete" @click="deleteItem">Удалить предмет</button></div>
      </div>
    </div>
  </div>
  <div>
    <div class="modal">
      <div class="left-content">
        <div class="divImgLeft"><img src="/public/img.png" alt="imageLeft" class="contentImg"/></div>
        <InfiniteSkeleton/>
      </div>
    </div>
  </div>
  
</div>
</template>

<style scoped>
  .main{
    background-color: var(--background-color);
    height: 100vh;
    transition: background-color 0.3s;
  }
  .flex-container{
    display: flex;
    justify-content: center;
    gap: 40px;
    height: 100vh;
    align-items: center;
    
  }
  .container{
    max-width: 1080px;
    margin: 0 auto;
  }
  .grid-cell {
  width: 150px;
  height: 150px;
  border: 1px solid rgba(77, 77, 77, 1);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.grid-cell.drag-over {
  transform: scale(1.1); 
}

.grid-cell img.dragging {
  opacity: 0.5;
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
  top: 106px;
  position: absolute;
  left: 970px;
  border: 1px solid rgba(77, 77, 77, 1);
  width: 350px;
  height: 758px;
  background-color: gray;
  border-radius: 0px 15px 15px 0px;
  background-color: var(--background-color);
  transform: translateX(-150%);
  animation: ani 1s forwards;

}

.border{
  margin: 15px 0px;
  border: 1px solid rgb(201, 198, 198);
}
.close {
  width: 1em;
  height: 1em;
  background-color: rgb(216, 216, 216);
  margin-top: 10px;
  margin-left: 320px;
  clip-path: polygon(20% 0%, 0% 20%, 30% 50%, 0% 80%, 20% 100%, 50% 70%, 80% 100%, 100% 80%, 70% 50%, 100% 20%, 80% 0%, 50% 30%);
  cursor: pointer;

}
.delete{
  width: 220px;
  border-radius: 11px;
  border: none;
  height: 39px;
  background-color: rgba(250, 114, 114, 1);
  color: white;
  font-size: 16px;
  cursor: pointer;
}
.btnFlex{
  display: flex;
  justify-content: center;
  align-items: end;
  height: 110px;
}
.delete:hover{
  opacity: 0.5;
}
.top-left {
  border-top-left-radius: 10px;
}
.top-right {
  border-top-right-radius: 10px;
}
.bottom-left {
  border-bottom-left-radius: 10px;
}
.bottom-right {
  border-bottom-right-radius: 10px;
}

@keyframes ani {
  0% {transform: translateX(25%);}
  100% {transform: translateY(0);}
}
.left-content{
  top: 106px;
  position: absolute;
  left: 170px;
  border: 1px solid rgba(77, 77, 77, 1);
  width: 350px;
  height: 758px;
  background-color: gray;
  border-radius: 15px 15px 15px 15px;
      background-color: var(--background-color);

}
.divImgLeft{
 display: flex;
 justify-content: center;
 height: 300px;
 align-items: center;

}

.theme-toggle {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: var(--delete-button-background-color);
  color: var(--text-color);
  border: none;
  padding: 10px;
  cursor: pointer;
}

.dark {
  --background-color: rgba(38, 38, 38, 1);
  --text-color: white;
  --border-color: rgba(77, 77, 77, 1);
  --modal-background-color: rgba(38, 38, 38, 1);
  --delete-button-background-color: rgba(250, 114, 114, 1);
}

.light {
  --background-color: white;
  --text-color: black;
  --border-color: rgba(200, 200, 200, 1);
  --modal-background-color: rgba(240, 240, 240, 1);
  --delete-button-background-color: rgba(255, 0, 0, 1);
}
</style>
