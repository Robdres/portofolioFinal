<script setup lang="ts">
import { reactive,watchEffect,shallowRef } from 'vue';
import { BasicShadowMap, SRGBColorSpace, NoToneMapping } from 'three';
import { extend,TresCanvas,useRenderLoop,useTexture } from '@tresjs/core';

import { TextGeometry } from 'three/addons/geometries/TextGeometry'
import { FontLoader } from 'three/addons/loaders/FontLoader'

import { OrbitControls,Text3D} from '@tresjs/cientos';

extend({ TextGeometry: TextGeometry })

const camera = {
  position: [2.789917807383607, 1.1520053956903624, 4.066760580672416],
  fov: 45,
  rotation: [-0.26033035821639566, 0.3493743923092192, 0.09093149349977715],
};

const letters = "abcdefghijklmnopqrstuvwxyz, R";
let interval = null

let func1 = (event) => {  
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
const textRef = shallowRef();
let widthBooks = 5
let heightBooks = 6
let lengthBooks = 2

let blue= "#112643";
let gold= "#D4AF37";
let gray= "#2F3D3B";
let plat= "#DBE0E3";

const boxRef: ShallowRef<TresInstance | null> = shallowRef(null);

//const { onLoop } = useRenderLoop();


const gl = {
  clearColor: plat,
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputColorSpace: SRGBColorSpace,
  toneMapping: NoToneMapping,
};
var tolerance = 0.00001;

function onClick(ev) {
  console.log(ev)
  if (ev) {
    if(1-ev.object.position.x<tolerance){
      let aux = setInterval(function() {
        ev.object.position.x -=0.1;
        if(ev.object.position.x<tolerance){
          clearInterval(aux)
        }
      }, 10);
    }else {
      let aux = setInterval(function() {
        ev.object.position.x +=0.1;
        if(1-ev.object.position.x<tolerance){
          clearInterval(aux)
        }
      }, 10);
    }
  }
}

</script>

<template #fallback>
<TresCanvas
    v-bind="gl" 
    clear-color="#282828"
    shadows
    alpha
    window-size
    physically-correct-lights
  >
    <TresPerspectiveCamera :position="[110, 0, 0]"  :look-at="[0, 0, 0]"/>
    <OrbitControls />
    <TresDirectionalLight :position="[100, 10, 10]" :intensity="2" cast-shadow />

    <TresGroup 
      :position="[0,0,0]"
      @click="(e)=>{console.log('hello')}"
    >
    <TresMesh :position="[0, 0, -3*lengthBooks]" cast-shadow
      @click="onClick"
      >
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, -2*lengthBooks]" cast-shadow
      @click="onClick"
      >
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh ref="boxRef" :position="[0, 0, -lengthBooks]" cast-shadow
      @click="onClick"
      >
      <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]"/>
      <TresMeshToonMaterial color="gray" />
    </TresMesh>
    <TresMesh  :position="[0, 0, 0]" cast-shadow
      @click="onClick"
      >
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, lengthBooks]" cast-shadow
      @click="onClick"
      >
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, 2*lengthBooks]" cast-shadow
      @click="onClick"
      >
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, 3*lengthBooks]" cast-shadow
      @click="onClick"
      >
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshNormalMaterial color="blue" />
    </TresMesh>
    </TresGroup>
  </TresCanvas>
  <h1 class="text" data-value="Hi I am," @mouseover="func1">
    Hi I am,
  </h1>
  <h1 class="name" data-value="Roberto Alvarado" @mouseover="func1">
    Roberto Alvarado 
  </h1>
  <span class="text">Gekki</span>
</template>

<style >
@import url('https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;500;700&display=swap');
html,
body {

  margin: 0;
  padding: 0;
  height: 100%;
  font-family: 'Rajdhani', sans-serif;
  width: 100%;
}
#app {
  height: 100%;
  width: 100%;
}
.text{
  padding: 3em;
  top: 50%;
  left: 50%;
  font-family: 'Rajdhani', sans-serif;
  position: absolute;
};

.text{
  transform: translate(-50%,-50%);
};

.name{
  color: red;

};

</style>

