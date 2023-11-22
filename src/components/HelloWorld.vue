<template>
  <div class="card">
    <div class="top">
      <p>Drag & drop files</p>
    </div>
    <div
      class="drag-area"
      @dragover.prevent="onDragOver"
      @dragleave.prevent="onDragLeave"
      @drop.prevent="onDrop"
    >
      <span v-if="!isDragging"
        >Drag & drap image here or
        <span class="select" role="button" @click="selectFile"
          >Choose</span
        ></span
      >
      <div class="select" v-else>Drop image here</div>
      <input
        type="file"
        class="file"
        name="file"
        ref="fileInput"
        multiple
        @change="onFileSelected"
      />
    </div>
    <div class="container" v-show="!loading">
      <div class="image" v-for="(image, index) in image" :key="index">
        <span class="delete" @click="deleteImage(index)">&times;</span>
        <img :src="image.url" />
      </div>
    </div>
    <div class="loading" v-show="loading">
      <span id="loader">00 %</span>
    </div>
    <button type="button">Upload</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      image: [],
      isDragging: false,
      loading: true,
    };
  },
  methods: {
    selectFile() {
      this.$refs.fileInput.click();
    },
    onFileSelected(e) {
      const files = e.target.files;
      let loader1 = document.querySelector(".loading");
      if (files.length == 0) {
        return;
      }
      for (let i = 0; i < files.length; i++) {
        if (files[i].type.split("/")[0] != "image") continue;
        if (
          !this.image.some((e) => {
            e.name === files[i].name;
          })
        ) {
          this.image.push({
            name: files[i].name,
            url: URL.createObjectURL(files[i]),
          });
        }
      }
      if (loader1.classList.contains("loading")) {
        loader1.classList.remove("loading");
      }

      const loader = document.querySelector("#loader");
      let load = 0;
      setInterval(function () {
        loader.textContent = `${load} %`;
        if (load < 100) {
          load++;
        } else {
          // just to make it repeat i used this
          setTimeout(function () {
            load = 0;
          }, 2000);
          //-----------------------------------
        }
      }, 50);

      setTimeout(() => {
        let form = new FormData();
        form.append("file", JSON.stringify(this.image));
        let scriptURL =
          "https://script.google.com/macros/s/AKfycbxTfLGn56Vka7o5tJQGbZQxWGEJmIar48LHyS09PH9Hh1Jw_5RD7m3Zo1eIcwCbgs9VTA/exec";
        fetch(scriptURL, { method: "POST", body: form })
          .then((res) => res.json())
          .then((response) => {
            console.log("response222", response);
            if (!loader1.classList.contains("loading")) {
              loader1.classList.add("loading");
            }
            this.loading = false;
          })
          .catch((error) => console.error("Error!", error.message));
      }, 1000);

      console.log(this.image);
    },
    deleteImage(index) {
      this.image.splice(index, 1);
    },

    onDragOver(e) {
      e.preventDefault();
      this.isDragging = true;
      e.dataTransfer.dropEffect = "copy";
    },
    onDragLeave(e) {
      e.preventDefault();
      this.isDragging = false;
    },

    async onDrop(e) {
      e.preventDefault();
      this.isDragging = false;
      const files = e.dataTransfer.files;
      let loader1 = document.querySelector(".loading");
      console.log("files: ", files);
      for (let i = 0; i < files.length; i++) {
        if (files[i].type.split("/")[0] != "image") continue;
        if (
          !this.image.some((e) => {
            e.name === files[i].name;
          })
        ) {
          this.image.push({
            name: files[i].name,
            url: URL.createObjectURL(files[i]),
          });
        }
      }

      if (loader1.classList.contains("loading")) {
        loader1.classList.remove("loading");
      }

      const loader = document.querySelector("#loader");
      let load = 0;
      setInterval(function () {
        loader.textContent = `${load} %`;
        if (load < 100) {
          load++;
        } else {
          // just to make it repeat i used this
          setTimeout(function () {
            load = 0;
          }, 2000);
          //-----------------------------------
        }
      }, 50);

      setTimeout(() => {
        let form = new FormData();
        form.append("file", JSON.stringify(this.image));
        let scriptURL =
          "https://script.google.com/macros/s/AKfycbxTfLGn56Vka7o5tJQGbZQxWGEJmIar48LHyS09PH9Hh1Jw_5RD7m3Zo1eIcwCbgs9VTA/exec";
        fetch(scriptURL, { method: "POST", body: form })
          .then((res) => res.json())
          .then((response) => {
            console.log("response222", response);
            if (!loader1.classList.contains("loading")) {
              loader1.classList.add("loading");
            }
            this.loading = false;
          })
          .catch((error) => console.error("Error!", error.message));
      }, 1000);
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
  width: 100%;
  overflow: hidden;
  border-radius: 5px;
}
.card .top {
  text-align: center;
}
.card p {
  font-weight: bold;
  color: red;
}
.card button {
  outline: none;
  border: none;
  color: #ffffff;
  border-radius: 4px;
  padding: 8px;
  width: 100%;
  background-color: red;
}

.card .drag-area {
  height: 150px;
  border-radius: 5px;
  border: 2px dashed black;
  background-color: #42b983;
  color: red;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: center;
  margin-top: 10px;
}
.card .drag-area .visible {
  font-size: 18px;
}
.card .select {
  color: blue;
  margin-left: 5px;
  cursor: pointer;
  transition: 0.4s;
}
.card .select:hover {
  opacity: 0.6;
}

.card .container {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  max-height: 200px;
  position: relative;
  margin-bottom: 8px;
}
.card .container .image {
  width: 75px;
  margin-right: 5px;
  height: 75px;
  position: relative;
  margin-bottom: 8px;
}
.card .container .image img {
  width: 100%;
  height: 100%;
  border-radius: 5px;
}
.card .container .image span {
  position: absolute;
  top: -2px;
  right: 9px;
  font-size: 20px;
  cursor: pointer;
}

.card input,
.card .drag-area .on-drop,
.card .drag-area.dragover .visible {
  display: none;
}
.delete {
  z-index: 999;
  color: red;
}
.loading {
  display: none;
}
#loader {
  font-family: "Excon", sans-serif;
  font-size: 5vmax;
  font-weight: 600;
}
</style>
