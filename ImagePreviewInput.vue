<template>
  <div>
    <b-form-file
      ref="file-input"
      class="mb-2"
      :placeholder="value.nome || 'Escolhar um arquivo ou solte-o aqui...'"
      drop-placeholder="Solte o arquivo aqui..."
      @change="previewImage"
      alt="Image de Perfil"
      accept="image/*"
      lang="pt"
    ></b-form-file>
    <transition enter-active-class="animated zoomIn" leave-active-class="img-leave">
      <div
        v-if="selectedFile || value.path"
        class="d-flex justify-content-center align-items-center border py-3 px-4 img-container position-relative"
      >
        <b-img :src="selectedFile || value.path" class="img" fluid thumbnail />
        <b-button variant="danger" class="remove-button" @click="removeImage">
          <fa-icon icon="trash"></fa-icon>
        </b-button>
      </div>
    </transition>
  </div>
</template>
<script>
export default {
  props: {
    value: {
      default() {
        return {};
      }
    }
  },

  data() {
    return {
      selectedFile: null,
      selectedFilename: ""
    };
  },

  // LIFECICLE METHODS -------------------------------
  created() {
    this.load();
  },

  computed: {},

  methods: {
    load() {},

    previewImage(event) {
      var input = event.target;
      if (input.files && input.files[0]) {
        var reader = new FileReader();
        reader.onload = e => {
          this.selectedFile = e.target.result;
          this.selectedFilename = event.target.files[0].name;
          this.value.base64 = this.selectedFile;
          this.value.nome = this.selectedFilename;
        };
        reader.readAsDataURL(input.files[0]);
      }
    },

    removeImage() {
      this.selectedFile = null;
      this.value.base64 = null;
      this.value.nome = "";
      this.value.path = "";

      this.$refs["file-input"].reset();
    }
  },

  watch: {
    value(val) {
      this.value = val;
      this.selectedFilename = val.nome;
    }
  }
};
</script>

<style>
.img-container {
  /* background-color: gray; */
  max-width: 100%;
  max-height: 40vh;
  min-height: 40vh;
}

.img {
  padding: 10px;
  max-width: 100%;
  max-height: 38vh;
  height: 100%;
}

.remove-button {
  position: absolute;
  right: 10px;
  top: 10px;
}

.custom-file-input:lang(pt) ~ .custom-file-label::after {
  content: "Carregar";
}

/* keyframes */
@keyframes img-out {
  from {
    min-height: 40vh;
    height: 40vh;
  }
  to {
    height: 0;
    min-height: 0;
    transform: translateX(100vw);
    opacity: 0;
  }
}

.img-leave {
  animation: img-out 1s ease-in;
}
</style>