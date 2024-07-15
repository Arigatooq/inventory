<template>
    <div class="main_flex">
        <div class="skeleton-cell"></div><br>
        <div class="skeleton-cell-2"></div><br>
        <div class="skeleton-cell-3"></div><br>
        <div class="skeleton-cell-4"></div><br>
        <div class="skeleton-cell-5"></div><br>
        <div class="skeleton-cell-6"></div><br>
        <div class="skeleton-cell-7"></div><br>
    </div>
  </template>
  
  <style scoped>
  .main_flex{
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }

  .skeleton-cell-2{
    border-radius: 8px;
    width: 250px;
    height: 15px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
    margin-top: 15px;
  }
  .skeleton-cell-3{
    border-radius: 8px;
    width: 310px;
    height: 18px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
  }
  .skeleton-cell-4{
    border-radius: 8px;
    width: 295px;
    height: 18px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
  }
  .skeleton-cell-5{
    border-radius: 8px;
    width: 275px;
    height: 18px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
  }
  .skeleton-cell-6{
    border-radius: 8px;
    width: 255px;
    height: 18px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
  }
  .skeleton-cell-7 {
    border-radius: 8px;
    width: 150px;
    height: 18px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;
    margin-top: 40px;
  }
  .skeleton-cell {
    border-radius: 8px;
    width: 300px;
    height: 30px;
    margin-right: 10px;
    background-color: #ccc;
    animation: pulse 1.5s infinite;

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
  