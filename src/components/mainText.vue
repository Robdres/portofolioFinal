<script>
const letters = "abcdefghijklmnopqrstuvwxyz, R";
let interval = null

let hover = (event) => {  
  let iteration = 0;
  clearInterval(interval);
  interval = setInterval(() => {
    event.target.innerText = event.target.innerText
      .split("")
      .map((letter, index) => {
        if(index < iteration) {
          return event.target.dataset.value[index];
        }
      
        return letters[Math.floor(Math.random() * 26)]
      })
      .join("");
    
    if(iteration >= event.target.dataset.value.length){ 
      clearInterval(interval);
    }
    iteration += 1 / 3;
  }, 20);
}
export default {
  name: 'MainTextComponent',
}
</script>

<template>
  <div class="main">
    <h1 class="text" data-value="Hi I am," @mouseover="hover">
      Hi I am,
    </h1>
    <h1 class="text" data-value="Roberto Alvarado" @mouseover="hover">
      Roberto Alvarado 
    </h1>
  </div>
</template>

<style>

.main{
  position: absolute;
  font-family: 'Rajdhani', sans-serif;
  -webkit-transform: translate(-50%,-50%);
  padding: 3em;
  top: 30%;
  left: 30%;
  text-align: left;
  border: red 1px solid;
};
.main .text{
  color:red;
};
</style>
