<script setup>
import { ref } from 'vue'
import TWrapper from './components/TWrapper.vue'
import CH1 from './components/CH1.vue'

const target = ref('mac')
const changeTarget = e => {
  target.value = e.target.value === 'mac' ? 'windows': 'mac'
}

const input = ref('')
const output = ref('')
const updateOutput = () => {
  if(target.value === 'mac') {
    output.value = input.value.replaceAll('\\', '/')
  }
  if(target.value === 'windows') {
    output.value = input.value.replaceAll('/', '\\')
  }
}

const writeClipboard = async () => {
  const result = await navigator.permissions.query({ name: "clipboard-write" })
  if (result.state === "granted" || result.state === "prompt") {
    navigator.clipboard.writeText(output.value)
    return true
  }
  return false
}

const handleClick = async () => {
  const success = await writeClipboard()
  if(success) {
    document.getElementById('dialog').showModal()
  }
}
</script>

<template>
  <TWrapper>
    <CH1 text="windows ⇄ mac 間のパス変換ツール"></CH1>
    <p class="p-text">/ と \ を変換するだけです。</p>
    <div class="p-col2">
      <div>
        <p class="p-title">
          <label for="select">変換前：</label>
          <select id="select" @change="changeTarget">
            <option value="mac" :selected="!(target === 'mac')">mac</option>
            <option value="windows" :selected="!(target === 'windows')">windows</option>
          </select>
        </p>
        <textarea rows="5" v-model="input" @input="updateOutput"></textarea>
      </div>
      <div>
        <p class="p-title">変換後：<span>{{ target }}</span><button v-if="(input !== '')" @click="handleClick" class="p-copyButton">コピー</button></p>
        <div class="p-output">
          <p>{{ output }}</p>
        </div>
      </div>
      <p></p>
    </div>
  </TWrapper>
  <dialog id="dialog">
    <form>
      <p>コピーしました！</p>
      <button formmethod="dialog">閉じる</button>
    </form>
  </dialog>
</template>

<style scoped>
.p-col2 {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
}
.p-title {
  margin: 32px 0 8px;
  font-weight: bold;
}
.p-text {
  margin-top: 32px;
}
.p-output {
  background-color: #fff;
  padding: 16px;
  min-height: 100px;
}
textarea {
  width: 100%;
  padding: 16px;
}
.p-copyButton {
  margin-left: 1rem;
}

dialog {
  position: fixed;
  top: 150px;
  left: 50%;
  transform: translateX(-50%);
}

@media screen and (max-width: 767px) {
  .p-col2 {
    grid-template-columns: 1fr;
  }
}
</style>