<template>
  <q-page
  ref="pageChat"
  class="flex column page-chat">
    <q-banner
      v-if="!otheruserDetails.online"
      inline-actions
      class="text-white bg-red"
    >
      {{ otheruserDetails.name }} не в сети
      <q-icon name="warning" />
    </q-banner>
    <div
    :class="{'invisible': !showMessages}"
    class="q-pa-md column col justify-end">
      <q-chat-message
        v-for="(message,key) in messages"
        :key="key"
        :name="message.from == 'Я' ? userDetails.name : otheruserDetails.name"
        :text="[message.text]"
        :sent="message.from == 'Я' ? true : false"
        :bg-color="message.from == 'Я' ? 'light-blue-4' : 'indigo-1'"
      />
    </div>
    <q-footer elevated>
      <q-toolbar>
        <q-form @submit="sendMessage" class="full-width">
          <q-input
            v-model="newMessage"
            label="Сообщение"
            bg-color="white"
            rounded
            outlined
            dense
          >
            <template v-slot:after>
              <q-btn
                round
                dense
                flat
                icon="send"
                color="white"
                @click="sendMessage()"
              />
            </template>
          </q-input>
        </q-form>
      </q-toolbar>
    </q-footer>
  </q-page>
</template>

<script>
import { mapState, mapActions } from "vuex";
import mixinOtherUserDetails from "src/mixins/mixin-other-user-details.js";
export default {
  mixins: [mixinOtherUserDetails],
  data() {
    return {
      newMessage: "",
      showMessages: false
    };
  },
  computed: {
    ...mapState("store", ["messages", "userDetails"])
  },
  methods: {
    ...mapActions("store", [
      "firebaseGetMessages",
      "firebaseStopGettingMessages",
      "firebaseSendMessage"
    ]),
    sendMessage() {
      this.firebaseSendMessage({
        message: {
          text: this.newMessage,
          from: "Я"
        },
        otherUserId:this.$route.params.otherUserId
      });
      this.newMessage = "";
    },
    scrollToBottom(){
      let pageChat = this.$refs.pageChat.$el
      setTimeout(() =>{
      window.scrollTo(0,pageChat.scrollHeight)
      },20)

    }
  },
  watch:{
    messages: function(val){
      if(Object.keys(val).length){
        this.scrollToBottom()
        setTimeout(()=>{
          this.showMessages = true
        },50)
      }
    }
  },
  mounted() {
    this.firebaseGetMessages(this.$route.params.otherUserId);
  },
  destroyed() {
    this.firebaseStopGettingMessages();
  }
};
</script>
<style scoped>
.page-chat{
 background:
linear-gradient(27deg, #151515 5px, transparent 5px) 0 5px,
linear-gradient(207deg, #151515 5px, transparent 5px) 10px 0px,
linear-gradient(27deg, #222 5px, transparent 5px) 0px 10px,
linear-gradient(207deg, #222 5px, transparent 5px) 10px 5px,
linear-gradient(90deg, #1b1b1b 10px, transparent 10px),
linear-gradient(#1d1d1d 25%, #1a1a1a 25%, #1a1a1a 50%, transparent 50%, transparent 75%, #242424 75%, #242424);
background-color: #131313;
background-size: 20px 20px;
}
</style>
