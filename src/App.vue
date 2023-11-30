<template>
  <div>
    <input v-model="pdfUrl" type="text" placeholder="Enter PDF URL" />
    <button @click="showPdfPreview">Show Preview</button>

    <div v-if="showPreview" class="popup-overlay" @click="closePreview">
      <div class="popup" ref="popup">
        <button @click="closePreview" class="close-btn">X</button>
        <iframe :src="pdfUrl" width="800rem" height="700rem"></iframe>
       
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pdfUrl: '',
      showPreview: false,
      initialWidth: 0,
      initialHeight: 0,
      mouseX: 0,
      mouseY: 0
    };
  },
  methods: {
    showPdfPreview() {
      if (this.pdfUrl) {
        this.showPreview = true;
      }
    },
    closePreview() {
      this.showPreview = false;
    },
    startResize(e) {
      e.preventDefault();
      this.isResizing = true;
      this.initialWidth = this.$refs.popup.offsetWidth;
      this.initialHeight = this.$refs.popup.offsetHeight;
      this.mouseX = e.clientX;
      this.mouseY = e.clientY;
      document.addEventListener('mousemove', this.resize);
      document.addEventListener('mouseup', this.stopResize);
    },
    resize(e) {
      if (this.isResizing) {
        const width = this.initialWidth + (e.clientX - this.mouseX);
        const height = this.initialHeight + (e.clientY - this.mouseY);
        this.$refs.popup.style.width = `${width}px`;
        this.$refs.popup.style.height = `${height}px`;
      }
    },
    stopResize() {
      this.isResizing = false;
      document.removeEventListener('mousemove', this.resize);
      document.removeEventListener('mouseup', this.stopResize);
    }
  }
};
</script>

<style>
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 200%;
  height: 200%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 999;
}

.popup {
  position: fixed;
  top: 23rem;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 1px;
  z-index: 1000;
  overflow: auto;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.resize-handle {
  width: 10px;
  height: 10px;
  background-color: #000;
  position: absolute;
  bottom: 0;
  right: 0;
  cursor: nwse-resize;
}
</style>
