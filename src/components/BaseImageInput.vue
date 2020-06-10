<template>
  <div
    class="base-image-input"
    @click="chooseImage"
  >
    <span
      v-if="modelLoaded"
      class="placeholder"
    >
      Choose an Image
    </span>
    <input
      class="file-input"
      ref="fileInput"
      type="file"
      @input="onSelectFile"
    >
    <p>{{predictionText}}</p>
    <div id="imageContainer">
    </div>
  </div>
</template>

<script>
import * as cvstfjs from '@microsoft/customvision-tfjs';

export default {
  data () {
    return {
      model: null,
      modelLoaded: false,
      predictionText: ""
    }
  },
  mounted () {
      (async() =>
      {
        this.model = new cvstfjs.ClassificationModel();
        //await this.model.loadModelAsync('https://authoringresources.blob.core.windows.net/image-recognition-model/model.json');
        await this.model.loadModelAsync('http://localhost:8080/model/model.json');
        this.modelLoaded = true;
      })();
      
  },
  methods: {
    chooseImage () {
        this.$refs.fileInput.click();
        this.predictionText = "processing...";
    },
    onSelectFile () {
        const input = this.$refs.fileInput
  const files = input.files
  if (files && files[0]) {
    const reader = new FileReader
    reader.onload = async (e) => 
    {
        const container = document.getElementById("imageContainer");
        container.innerHTML = "";
        const img = document.createElement("img");
        container.appendChild(img);
        img.src = e.target.result;
        

        await this.sleep(10);
        const result = await this.model.executeAsync(img);
        var isComplex = result[0][0] > .7;
        console.log(result);

        this.predictionText = (isComplex) ? "This image is complex!" : "This image is NOT complex!";
    }
        reader.readAsDataURL(files[0])
        this.$emit('input', files[0])
    }
    },
    sleep (ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

  }
}
</script>

<style scoped>
.base-image-input {
  display: block;
  width: 200px;
  height: 200px;
  cursor: pointer;
  background-size: cover;
  background-position: center center;
}
.placeholder {
  background: #F0F0F0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #333;
  font-size: 18px;
  font-family: Helvetica;
}
.placeholder:hover {
  background: #E0E0E0;
}
.file-input {
  display: none;
}
</style>