<script setup lang="ts">
import { reactive,ShallowRef,shallowRef } from 'vue';
import { BasicShadowMap, SRGBColorSpace, NoToneMapping } from 'three';
import { TresCanvas,useRenderLoop } from '@tresjs/core';

import { OrbitControls } from '@tresjs/cientos';

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
    if(1-ev.object.position.y<tolerance){
      let aux = setInterval(function() {
        ev.object.position.y -=0.1;
        if(ev.object.position.y<tolerance){
          clearInterval(aux)
        }
      }, 10);
    }else {
      let aux = setInterval(function() {
        ev.object.position.y +=0.1;
        if(1-ev.object.position.y<tolerance){
          clearInterval(aux)
        }
      }, 10);
    }
  }
}
function onClick1(ev) {
  console.log(ev)
  if (ev) {
    if(1-ev.object.position.y<tolerance){
      let aux = setInterval(function() {
        ev.object.position.y -=0.1;
        if(ev.object.position.y<tolerance){
          clearInterval(aux)
        }
      }, 10);
    }else {
      let aux = setInterval(function() {
        ev.object.position.y +=0.1;
        if(1-ev.object.position.y<tolerance){
          clearInterval(aux)
        }
      }, 10);
    }
  }
}

</script>

<template>
  <TresCanvas v-bind="gl" window-size>
    <TresPerspectiveCamera :position="[15, 0, 0]"  :look-at="[0, 0, 0]"/>
    <OrbitControls/>
    <TresGroup 
      @click="(e)=>{console.log(e)}"
    >
    <TresMesh :position="[0, 0, -3*lengthBooks]" cast-shadow>
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, -2*lengthBooks]" cast-shadow>
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh ref="boxRef" :position="[0, 0, -lengthBooks]" cast-shadow
      @pointer-enter="onClick"
      @pointer-leave="onClick1"
      >
      <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]"/>
      <TresMeshToonMaterial color="gray" />
    </TresMesh>
    <TresMesh  :position="[0, 0, 0]" cast-shadow>
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, lengthBooks]" cast-shadow>
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, 2*lengthBooks]" cast-shadow>
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshToonMaterial color="blue" />
    </TresMesh>
    <TresMesh :position="[0, 0, 3*lengthBooks]" cast-shadow>
    <TresBoxGeometry :args="[widthBooks, heightBooks, lengthBooks]" />
      <TresMeshNormalMaterial color="blue" />
    </TresMesh>
    </TresGroup>
    <TresDirectionalLight :position="[0, 15, 15]" :look-at="[0,0,0]" :intensity="1" cast-shadow />
  </TresCanvas>
</template>


<style>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}
#app {
  height: 100%;
  width: 100%;
}
</style>
