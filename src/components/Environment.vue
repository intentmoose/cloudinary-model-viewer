<template>
  <a-entity light="type: ambient; color: #BBB"></a-entity>

  <!-- Shadow radius gives the shadow the soft falloff -->
  <a-entity
    light="type: directional; 
    color: #FFF; 
    intensity: 0.6; 
    castShadow:true; 
    shadowRadius: 10;
    shadowCameraRight: 8;
    shadowCameraLeft: -8;"
    position="-0.5 2 1"
    @loaded="directionalLoaded"
  ></a-entity>

  <a-plane
    shadow="receive: true;"
    material="shader: shadow"
    width="40"
    height="40"
    rotation="-90 0 0"
    position="0 -3 0"
  ></a-plane>

  <a-entity
    camera
    look-controls
    orbit-controls="target: 0 0 0; 
    minDistance: 0.5; 
    maxDistance: 180; 
    initialPosition: 0 0 25; 
    dampingFactor: 0.04; 
    enablePan: false;"
  ></a-entity>
</template>

<script>
export default {
  name: "Environment",
};
</script>
