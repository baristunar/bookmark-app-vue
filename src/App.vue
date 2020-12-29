<template>
  <div>
    <header>Bookmark App</header>

    <addition-side
      :bookmark="bookmark"
      @send-data-event="saveData"
      :updateStatus="updateStatus"
    />
    <bookmark-container
      :bookmarkList="bookmarkList"
      @delete-data-event="deleteData"
      :updateID="updateID"
      :updateStatus="updateStatus"
      @update-item-event="updateItem"
      :bookmark="bookmark"
      @cancel-update-event="cancelUpdate"
      @update-data-event="updateData"
    />
  </div>
</template>

<script>
import additionSide from "../components/additionSide.vue";
import bookmarkContainer from "../components/bookmarkContainer.vue";

import axios from "axios";

export default {
  components: {
    additionSide,
    bookmarkContainer,
  },

  data() {
    return {
      bookmarkList: [],
      bookmark: {
        
        title: null,
        description: null,
        url: null,
        updateStatus: false,
      },
      updateID: null,
      updateStatus: false,
    };
  },
  methods: {
    saveData() {
      axios
        .post("http://localhost:3000/bookmarks", this.bookmark)
        .then((bookmark_save_response) => {
          console.log("bookmark_save_response", bookmark_save_response);
          // Created...
          if (bookmark_save_response.status === 201) {
            this.bookmarkList.push(bookmark_save_response.data);
            this.bookmark = {
              title: null,
              description: null,
              url: null,
              updateStatus: false,
            };
          }
        });
    },
    deleteData(id) {
      axios
        .delete(`http://localhost:3000/bookmarks/${id}`)
        .then((bookmark_delete_response) => {
          console.log("bookmark_delete_response", bookmark_delete_response);
          this.bookmarkList = this.bookmarkList.filter((b) => b.id !== id);
        });
    },
    updateData() {
      axios
        .patch(
          `http://localhost:3000/bookmarks/${this.updateID}`,
          this.bookmark
        )
        .then((update_response) => {
          const matchedBookmark = this.bookmarkList.findIndex(
            (b) => b.id === this.updateID
          );
          const getTrue = this.bookmarkList.findIndex(
            (b) => b.updateStatus == true
          );
          this.bookmarkList[getTrue].updateStatus = false;
          if (matchedBookmark > -1) {
            this.bookmarkList[matchedBookmark] = {
              ...this.bookmark,
            };

            this.updateStatus = false;
            this.updateID = null;
            this.bookmark = {
              title: null,
              description: null,
              url: null,
              updateStatus: false,
            };
            console.log(
              "updateStatus => ",
              this.updateStatus,
              "updateID=>",
              this.updateID
            );
          }
        });
    },
    updateItem(bookmark_item) {
      console.log("bookmark", bookmark_item);
      bookmark_item.updateStatus = true;

      this.bookmark = {
        id:bookmark_item.id,
        title: bookmark_item.title,
        description: bookmark_item.description,
        url: bookmark_item.url,
      };

      this.updateID = bookmark_item.id;
      this.updateStatus = true;
      console.log(
        "updateStatus => ",
        this.updateStatus,
        "updateID=>",
        this.updateID
      );
    },
    cancelUpdate() {
      const getTrue = this.bookmarkList.findIndex(
        (b) => b.updateStatus == true
      );
      this.bookmarkList[getTrue].updateStatus = false;
      this.updateStatus = false;
      this.bookmark = {
        title: null,
        description: null,
        url: null,
        updateStatus: false,
      };
    },
  },
  created() {
    axios
      .get("http://localhost:3000/bookmarks")
      .then((bookmark_response) => {
        console.log("bookmark_response ", bookmark_response);
        this.bookmarkList = bookmark_response.data;
      })
      .catch((e) => {
        console.log("Error", e);
      });
  },
};
</script>

<style scoped>
.form-container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
</style>
