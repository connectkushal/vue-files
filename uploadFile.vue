<template>
  <div class="file">
    <label class="file-label">
      <input
        class="file-input"
        type="file"
        :name="fileInputName"
        id="file"
        ref="file"
        v-on:change="handleFileUpload"
      />
      <span class="file-cta">
        <span class="file-icon">
          <font-awesome-icon :icon="['fas', 'file-upload']" />
        </span>
        <span class="file-label"> Choose a file… </span>
      </span>
    </label>
  </div>
</template>
<script>
import { ref } from "@vue/reactivity";
import axios from "axios";
export default {
  name: "uploadFile",
  props: {
    url: {
      type: String,
      required: true,
    },
    fileInputName: {
      type: String,
      required: true,
    },
  },

  setup(props) {
    const file = ref({});

    function handleFileUpload() {
      console.log(props.fileInputName);
      console.log("file upload clicked");

      var fileToUpload = file.value.files[0];
      console.log(fileToUpload);

      var formData = new FormData();
      formData.append(props.fileInputName, fileToUpload);
      //console.log(formData.getAll(props.fileInputName));

      axios
        .post(props.url, formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then((res) => {
          // Check for status headers, and show message accrdingly, eg empty file bad request etc
          console.log({ res });
        })
        .catch((err) => {
          console.error({ err });
        });
    }

    return { handleFileUpload, file };
  },
};

/* 
  - Depends on axios, bulma and font-awesome for Vue3
  - Pass the post endpoint url for file upload and the input name same as that on the api
*/
</script>
