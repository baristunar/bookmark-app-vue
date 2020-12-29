<template>
  <div>
    <div v-if="!updateStatus" class="container">
      <div class="form-group mr-5">
        <label>Başlık</label>
        <input v-model="bookmark.title" type="text" class="form-control" />
      </div>
      <div class="form-group mr-5">
        <label>Açıklama</label>
        <input
          v-model="bookmark.description"
          type="text"
          class="form-control"
        />
      </div>
      <div class="form-group mr-5">
        <label>URL</label>
        <input v-model="bookmark.url" type="text" class="form-control" />
      </div>
      <button v-if="!updateStatus" @click="sendData" class="btn-primary mt-10">
        Ekle
      </button>
    </div>
  </div>
</template>




<script>
export default {
  props: ["bookmark", "updateStatus"],
  data() {
    return {
      
    };
  },
  methods: {
    sendData() {
      this.$emit('send-data-event');
    },
  },
};
</script>