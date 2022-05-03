<template>
  <div class="container mt-5">
    formState:{{ formState }}
    <form enctype="multipart/form-data">
      <div class="form-group my-2">
        <label for="exampleInputEmail1">File Name</label>
        <input class="form-control" v-model="formState.name">
      </div>

      <div class="form-group my-2">
        <label class="form-label" for="customFile">Upload Store Documents</label>
        <input type="file" class="form-control" id="storeFile"
          @change="appendFile(event.target.name, event.target.files)" />
      </div>

      <button type="submit" class="btn btn-primary" @click.prevent="createStoreDoc">Submit</button>
    </form>

    <h1>Store Documents</h1>
    <div v-if="storeDoc">
      <div v-for="doc in storeDoc" :key="doc.id">
        <div>
          File ID:{{ doc.id }}
        </div>
        <div>
          File Name:{{ doc.name }}
        </div>
        <div>
          File Content:{{ doc.file }}
        </div>
        <hr>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, toRaw, reactive } from "vue";

const formState = reactive({
  name: "",
  file: null,
});


const apiClient = axios.create({
  baseURL: "http://127.0.0.1:8000/api/store-doc/",
  withCredentials: false,
  headers: {
    // Accept: "application/json",
    "Content-Type": "application/json",
  },
});

const storeDoc = ref(null)
const getStoreDoc = () => {
  apiClient.get()
    .then((response) => {
      storeDoc.value = response.data
      // console.log(storeDoc)
    })
}
getStoreDoc()


const createStoreDoc = () => {
  apiClient.post('', toRaw(formState))
    .then((response) => {
      console.log("success!" + response.data);
      getStoreDoc()
    })
    .catch((error) => {
      console.log(error);
    });
}



</script>

<style>
</style>
