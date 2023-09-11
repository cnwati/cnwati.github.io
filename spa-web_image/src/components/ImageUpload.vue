<template>
    <div>
      <div class="card">
        <div class="card-header">
            Upload Image
        </div>
        <div class="card-body">
          <input type="file" @change="selectImage" accept="image/*" />
          <button @click="uploadImage" class="btn btn-primary">Upload</button>
          <p v-if="uploadSuccess">{{ uploadMessage }}</p>
          <p v-if="uploadError">{{ uploadMessage }}</p>
        </div>
      </div>
    </div>
</template>
  
  <script>
  export default {
    name: 'ImageUpload',
    data() {
      return {
        selectedImage: null,
        uploadSuccess: false,
        uploadError: false,
        uploadMessage: "",
        apiKey: "50a9efaa0e05ce67d8ac99d6ddaa6cda", // Gantilah dengan API Key Anda
      };
    },
    methods: {
      selectImage(event) {
        this.selectedImage = event.target.files[0];
      },
      async uploadImage() {
        if (this.selectedImage) {
          const formData = new FormData();
          formData.append("image", this.selectedImage);
  
          try {
            const response = await this.uploadToImgBB(formData);
  
            if (response.data.status === 200) {
              this.uploadSuccess = true;
              this.uploadError = false;
              this.uploadMessage = "Gambar berhasil diunggah. URL: " + response.data.data.url;
            } else {
              this.uploadSuccess = false;
              this.uploadError = true;
              this.uploadMessage = "Terjadi kesalahan saat mengunggah gambar.";
            }
          } catch (error) {
            console.error(error);
            this.uploadSuccess = false;
            this.uploadError = true;
            this.uploadMessage = "Terjadi kesalahan saat mengunggah gambar.";
          }
        } else {
          this.uploadSuccess = false;
          this.uploadError = true;
          this.uploadMessage = "Pilih gambar terlebih dahulu.";
        }
      },
      async uploadToImgBB(formData) {
        const apiKey = this.apiKey;
        const apiUrl = "https://api.imgbb.com/1/upload?key=" + apiKey;
  
        return await this.$axios.post(apiUrl, formData);
      },
    },
  };
  </script>
  

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.card {
    width: 30rem;
    margin: 0 auto;
}
</style>