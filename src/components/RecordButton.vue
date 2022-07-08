<script setup lang="ts">
import { onMounted, ref } from "vue";
// @ts-ignore
import { setup, isSupported } from "@lusun-sdk/recorder";
const recordButton = ref<HTMLButtonElement | null>(null);

onMounted(async () => {
  if (!recordButton.value) {
    return;
  }

  const { supported, error } = await isSupported();
  if (!supported) {
    // 浏览器不支持需要提示用户
    recordButton.value.onclick = () => {
      alert(error);
    };
  } else {
    const { configureButton } = await setup({
      appId: "6f2171a9-d31b-196c-15cd-84cbed312bfa",
      env: "dev",
    });

    const btn = configureButton({
      element: recordButton.value,
      label: "保存视频",
    });
    btn.on("insert-click", (video: { sharedUrl: string }) => {
      // 新窗口打开视频链接
      window.open(video.sharedUrl, "_blank");
    });
  }
});
</script>

<template>
  <button ref="recordButton">录制</button>
</template>
