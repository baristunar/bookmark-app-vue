<template>
  <div>
    <div class="bookmark--app--container">
      <div
        v-for="bookmark_item in bookmarkList"
        :key="bookmark_item.id"
        class="bookmark-item"
      >
        <div class="header">
          <h3 v-if="!bookmark_item.updateStatus">{{ bookmark_item.title }}</h3>
          <button
            v-if="!updateStatus"
            @click="deleteData(bookmark_item.id)"
            class="btn-sm btn-danger"
          >
            Kaydı Sil
          </button>
        </div>
        <p v-if="!bookmark_item.updateStatus" class="mt-20 mb-20">
          {{ bookmark_item.description }}
        </p>
        <div class="d-flex justify-content-between align-items-center">
          <a v-if="!bookmark_item.updateStatus" href="#">
            {{ bookmark_item.url }}
          </a>
          <button
            v-if="!updateStatus"
            @click="updateItem(bookmark_item)"
            class="btn-warning btn-sm"
          >
            Düzenle
          </button>
        </div>
        <input
          style="margin: 5px;"
          v-model="bookmark.title"
          v-if="bookmark_item.updateStatus"
          type="text"
          class="form-control"
        />
        <input
          style="margin: 5px;"
          v-model="bookmark.description"
          v-if="bookmark_item.updateStatus"
          type="text"
          class="form-control"
        />
        <input
          style="margin: 5px;"
          v-model="bookmark.url"
          v-if="bookmark_item.updateStatus"
          type="text"
          class="form-control"
        />
        <button
          style="margin: 5px;"
          v-if="updateStatus && bookmark_item.updateStatus"
          @click="updateData"
          class="btn-warning mt-10 mr-5"
        >
          Değiştir
        </button>
        <button
          style="margin: 5px;"
          v-if="updateStatus && bookmark_item.updateStatus"
          @click="$emit('cancel-update-event')"
          class="btn-default mt-10"
        >
          İptal
        </button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  props: ["bookmarkList", "updateStatus", "updateID", "bookmark"],
  data() {
    return {};
  },
  methods: {
    deleteData(id) {
      this.$emit("delete-data-event", id);
    },
    updateItem(bookmark) {
        this.$emit("update-item-event", bookmark);
     
    },
    updateData() {
      this.$emit("update-data-event");
    },
  },
};
</script>