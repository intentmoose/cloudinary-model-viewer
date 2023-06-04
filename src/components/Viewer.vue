<template>
  <div
    id="dropzone"
    @dragover.prevent
    @dragleave="isDragging = false"
    @dragenter="isDragging = true"
    @drop="handleDrop"
  >
    <div class="drag-overlay" v-if="isDragging">
      <h1>Drop a GLTF or GLB file here</h1>
    </div>

    <a-scene renderer="colorManagement: true" shadow="type: pcfsoft">
      <Environment />
      <Model :modelUrl="modelUrl" />
    </a-scene>
  </div>
</template>

<script>
import Environment from "@/components/Environment.vue";
import Model from "@/components/Model.vue";
export default {
  name: "Viewer",
  components: {
    Environment,
    Model,
  },
  data() {
    return {
      modelUrl: "tiny-misplaced-burger.glb",
      isDragging: false,
    };
  },
  methods: {
    handleDrop(e) {
      e.preventDefault();
      this.isDragging = false;
      let file = e.dataTransfer.files[0];
      if (file.name.endsWith(".gltf") || file.name.endsWith(".glb")) {
        let url = URL.createObjectURL(file);
        this.modelUrl = url;
      }
    },
  },
};
</script>


<style lang="scss" scoped>
.drag-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  border: 15px dashed #ccc;
}
</style>