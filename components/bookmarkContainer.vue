<template>
  <div>
    <div class="container">
      <!-- COMPONENT -->
      <update-side
        :bookmarkList="bookmarkList"
        @delete-data-event="deleteData"
        :updateID="updateID"
        :updateStatus="updateStatus"
        @update-item-event="updateItem"
        :bookmark="bookmark"
        @cancel-update-event="cancelUpdate"
        @update-data-event="updateData"
      />
      <!-- /COMPONENT -->
    </div>
  </div>
</template>


<script>
import updateSide from "../components/updateSide.vue";
export default {
  components: { updateSide },
  props: ["bookmarkList", "updateStatus", "updateID","bookmark"],
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
    cancelUpdate(){
      this.$emit('cancel-update-event');
    },
    updateData(){
      this.$emit('update-data-event')
    }
  },
  
};
</script>