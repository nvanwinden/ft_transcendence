<template>
  <h2>Public chats</h2>
  <div class="overflow-y">
    <div v-for="publicChat in publicChats" :key="publicChat.id">
      <div class="chat-message-box main-chat-page-format">
        <p class="chatroom-list">
          <a :href="publicChat.linkUrl">{{ publicChat.chatroomName }}</a>
          owned by
          {{ publicChat.owner.playerName }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import apiRequest from "@/utils/apiRequest";
import { ref, onMounted } from "vue";
import { frontendUrl } from "@/utils/apiRequest";

const publicChats = ref([]);
onMounted(async () => {
  await apiRequest("/chat/type/public", "get")
    .then((response) => {
      publicChats.value = response.data;
      for (const link of publicChats.value) {
        const linkUrl = frontendUrl + "/chat/" + link.id;
        link["linkUrl"] = linkUrl;
        link.owner.playerName =
          link.owner.playerName ?? "namelessPlayer" + link.owner.id;
      }
    })
    .catch((err) => console.error(err));
});
</script>
<style scoped>
h2 {
  font-size: 3rem;
  margin: 1% 0%;
}

.overflow-y {
  height: 20rem;
  overflow-y: auto;
}

a {
  color: aliceblue;
}
a:hover {
  color: #39ff14;
}
</style>
