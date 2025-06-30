<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
const urlTitle = ref('');
const urlPath = ref('');
const url_list = ref([]);

const addURL = () => {
  if (urlPath.value != '') {
    const newURL = {
      id: crypto.randomUUID(),
      title: urlTitle.value,
      url: urlPath.value,
    };
    url_list.value.unshift(newURL);
    localStorage.setItem('URLList', JSON.stringify(url_list.value));
    urlTitle.value = '';
    urlPath.value = '';
    console.log(url_list.value);
  }
};
onMounted(() => {
  const storage = localStorage.getItem('URLList');

  if (storage != null) {
    const result = JSON.parse(storage);
    url_list.value = result;
  }
});
const clearStorage = () => {
  url_list.value = [];
  localStorage.removeItem('URLList');
};

const deleteItem = (urlID) => {
  url_list.value = url_list.value.filter((item) => item.id != urlID);
  localStorage.setItem('URLList', JSON.stringify(url_list.value));
};
</script>

<template>
  <h1>收藏網址小工具</h1>
  <button @click="clearStorage">清除所有資料</button>
  <hr />
  <div>
    輸入網址標題
    <input v-model.trim="urlTitle" type="text" class="input" />
  </div>

  <div>
    輸入網址內容
    <input
      @keyup.enter="addURL"
      v-model.trim="urlPath"
      type="text"
      class="input"
    />
  </div>

  <button @click="addURL">新增收藏</button>
  <hr />
  <ul>
    <li v-for="url in url_list">
      {{ url.title }} {{ url.url }}
      <button @click="deleteItem(url.id)">刪除</button>
    </li>
  </ul>
</template>
