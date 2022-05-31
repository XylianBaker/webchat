<template>
  <div class="absolute z-10 grid w-full place-items-center">
    <div id="app">
      <h2 class="mt-20 text-2xl text-white">Webchat App 🤯</h2>
      <div class="my-20">
        <label
          for="default-input"
          class="block mb-2 text-sm font-medium text-gray-900  dark:text-gray-300"
          >Message</label
        >
        <input
          type="text"
          v-model="message"
          id="default-input"
          class="
            bg-gray-50
            border border-gray-300
            text-gray-900 text-sm
            rounded-lg
            focus:ring-blue-500 focus:border-blue-500
            block
            w-full
            p-2.5
            dark:bg-gray-700
            dark:border-gray-600
            dark:placeholder-gray-400
            dark:text-white
            dark:focus:ring-blue-500
            dark:focus:border-blue-500
          "
        />
      </div>
      <button
        @click="sendMessage()"
        class="
          text-white
          bg-gradient-to-r
          from-blue-500
          via-blue-600
          to-blue-700
          hover:bg-gradient-to-br
          focus:ring-4 focus:outline-none focus:ring-blue-300
          dark:focus:ring-blue-800
          shadow-lg shadow-blue-500/50
          dark:shadow-lg dark:shadow-blue-800/80
          font-medium
          rounded-lg
          text-sm
          px-5
          py-2.5
          text-center
          mr-2
          mb-20
        "
      >
        Send Message
      </button>
    </div>
    <div v-for="msg in this.messages" :key="msg">
      <ul
        class="p-2 my-2 text-center text-white rounded-lg  bg-gradient-to-r from-cyan-500 to-blue-500"
      >
        {{
          msg
        }}
      </ul>
    </div>
  </div>
</template>

  <script>
import { io } from "socket.io-client";
export default {
  name: "App",
  data: function () {
    return {
      connection: null,
      message: "",
      messages: [],
      join: "",
      socket: io("http://localhost:3000"),
    };
  },
  mounted() {
    this.socket.on("my broadcast", (data) => {
      console.log(data);
      this.messages.push(data);
    });
  },
  beforeUnmount() {
    if (this.socket) {
      this.socket.disconnect();
    }
  },
  methods: {
    sendMessage: function () {
      this.socket.emit("my message", this.message);
      this.message = "";
    },
  },
};
</script>