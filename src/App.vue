<template>
  <form id="app" v-on:submit.prevent="submitForm" enctype="multipart/form-data">
    <div class="form-group">
      <label for="files">files</label>
      <input
        type="file"
        id="files"
        name="files"
        multiple
        class="form-control"
        @change="onFileChange"
      />
    </div>

    <div class="form-group">
      <label for="pictures">pictures</label>
      <input
        type="file"
        id="pictures"
        name="pictures"
        multiple
        class="form-control"
        @change="onPictureChange"
      />
    </div>
    <div class="form-group">
      <button class="btn btn-primary">Submit</button>
    </div>
  </form>
</template>

<script>
import CREATE_PROJECT from "./graphql/createProject";

export default {
  name: "App",
  data() {
    return {
      name: "مشروع تجربة رفع الصور",
      projectType: "شقق-سكنية",
      projectStatus: "open",
      units: 1,
      startDate: "2021-12-23",
      endDate: "2022-01-01",
      investors: ["zestycow8"],
      supervisors: ["wearypie2"],
      description: "",
      files: [],
      pictures: []
    };
  },

  methods: {
    async onFileChange({ target }) {
      var files = target.files;
      if (!files.length) return;
      for (let i = 0; i < files.length; i++) {
        this.files.push(files[i]);
      }
    },

    async onPictureChange({ target }) {
      var pictures = target.files;
      if (!pictures.length) return;
      for (let i = 0; i < pictures.length; i++) {
        this.pictures.push(pictures[i]);
      }
    },

     async submitForm() {
       console.log(this.files[0])
       await this.$apollo.mutate({
        mutation: CREATE_PROJECT,
        variables: {
          name: this.name,
          projectType: this.projectType,
          projectStatus: this.projectStatus,
          units: this.units,
          startDate: this.startDate,
          endDate: this.endDate,
          investors: this.investors,
          supervisors: this.supervisors,
          description: this.description,
          files: this.files,
          pictures: this.pictures
        },
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
