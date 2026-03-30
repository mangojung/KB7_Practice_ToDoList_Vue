<template>
  <div
    style="margin: 0 10% 4px 10%; display: flex; justify-content: space-between"
  >
    <span>전체 todo 개수: {{ cnt[0] }}</span>
    <span>완료 todo 개수: {{ cnt[1] }}</span>
    <span>미완료 todo 개수: {{ cnt[2] }}</span>
  </div>
</template>

<script setup>
defineProps({
  cnt: {
    type: Array,
    required: true,
  },
});
</script>
