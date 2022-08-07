<template>
  WORKING TIME<input type="text" v-model="workingTime">
  REST TIME<input type="text" v-model="restTime">
  PID<input type="text" v-model="pid">
  <button @click="startConnection">start connection</button>
  <button @click="createMessage">create</button>
  <button @click="joinMessage">join</button>
  <button @click="syncMessage">sync</button>
</template>
<script setup lang="ts">
import { ref, reactive, computed, watchEffect } from 'vue'

const pid = ref('')
const workingTime = ref(0)
const restTime = ref(0)
const connection = ref<WebSocket>()
const workCount = ref()
const restCount = ref()
const setWorkTime = ref()
const setRestTime = ref()
const isMaster = ref(false)

const startConnection = () => {
  connection.value = new WebSocket("ws://localhost:8080");
  //接続通知
  connection.value.onopen = function (event) {
    console.log(event)
  };

  //エラー発生
  connection.value.onerror = function (error) {
    console.log(error)
  };

  //メッセージ受信
  connection.value.onmessage = function (event) {
    console.log(event)
    const obj = JSON.parse(event.data)
    if (obj.message === "create") {
      isMaster.value = true
      console.log("success create!")
      pid.value = obj.pid
      countDown()
    }
    if (!isMaster.value && obj.message === "sync") {
      console.log("sync!")
      workingTime.value = obj.workingTime
      restTime.value = obj.restTime
    }
  };

  //切断
  connection.value.onclose = function () {
    console.log("close")
  };
}

const countDown = () => {
  workCount.value = setInterval(() => {
    if (workingTime.value === -1) return;
    if (workingTime.value === 0) {
      workingTime.value = -1
      restTime.value = setRestTime.value
      return;
    }
    workingTime.value = workingTime.value - 1
  }, 1000)
  restCount.value = setInterval(() => {
    if (restTime.value === -1) return;
    if (restTime.value === 0) {
      restTime.value = -1
      workingTime.value = setWorkTime.value
      return;
    }
    restTime.value = restTime.value - 1
  })
}

const createMessage = () => {
  const obj = {
    message: "create",
    workingTime: 30,
    restTime: 10
  }
  workingTime.value = 30
  restTime.value = -1
  setWorkTime.value = 30
  setRestTime.value = 10
  connection.value?.send(JSON.stringify(obj))
}

const joinMessage = () => {
  const obj = {
    message: "join",
    pid: pid.value
  }
  connection.value?.send(JSON.stringify(obj))
}

// 1秒ごとにカウントダウンしながら送る
const syncMessage = () => {
  setInterval(() => {
    const obj = {
      pid: pid.value,
      message: "sync",
      workingTime: workingTime.value,
      restTime: restTime.value
    }
    connection.value?.send(JSON.stringify(obj))
  }, 1000)
}

</script>