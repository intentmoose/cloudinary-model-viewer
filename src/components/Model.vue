<template>
  <a-entity
    ref="model"
    :gltf-model="modelUrl"
    @model-loaded="modelLoaded"
    shadow="receive: false; cast: true"
  ></a-entity>
</template>

<script>
export default {
  name: "Model",
  props: {
    modelUrl: {
      type: String,
      required: true,
    },
  },
  watch: {
    modelUrl(event) {
      // After drag and drop, reset scale before resizing
      this.$refs.model.object3D.scale.set(1, 1, 1);
    },
  },
  methods: {
    modelLoaded(event) {
      let model = event.target.object3D;

      this.centerModel(model);

      this.adjustSize(model);
    },
    getBoundingBox(object) {
      let boundingBox = new THREE.Box3();
      boundingBox.setFromObject(object);
      return boundingBox;
    },
    adjustSize(object) {
      // Too large or too small models may not be visible so we scale them to a reasonable size
      let boundingBox = this.getBoundingBox(object);
      let size = boundingBox.getSize(new THREE.Vector3());

      let maxDimension = Math.max(size.x, size.y, size.z);

      let scaleFactor = 10 / maxDimension;

      object.scale.set(scaleFactor, scaleFactor, scaleFactor);
    },
    centerModel(object) {
      // Models may be off center so we center them according the the bounding box
      let boundingBox = this.getBoundingBox(object);

      let center = new THREE.Vector3();
      boundingBox.getCenter(center);

      let translation = center.negate();

      object.traverse((node) => {
        if (node.isMesh) {
          node.position.add(translation);
        }
      });
    },
  },
};
</script>