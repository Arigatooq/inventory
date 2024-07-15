<template>
    <div class="main_flex">
        <div class="main-skeleton">
            <div class="skeleton-cell"></div>
        </div>
    </div>
  </template>
  

  <style scoped>
  .main_flex{
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }

  .skeleton-cell{
    border-radius: 8px;
    width: 1050px;
    height: 35px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
    margin-top: 8px;
    margin-left: 10px;
    
  }
  .main-skeleton{
    border: 1px solid rgba(77, 77, 77, 1);
    position: absolute;
    left: 170px;
    top: 880px;
    height: 52px;
    border-radius: 12px;
    width: 1162px;
  }
 
  @keyframes pulse {
    0% {
      background-color: rgba(60, 60, 60, 1);
    }
    50% {
      background-color: rgba(68, 68, 68, 1);
    }
    100% {
      background-color: rgba(51, 51, 51, 1);
    }
  }
  </style>

  
  