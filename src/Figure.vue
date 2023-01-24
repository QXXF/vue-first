<script setup lang="ts">
    defineProps<{ data: any }>()
</script>

<template>
    <picture>
        <img :src="data?.img" :alt="data?.body" :title="data?.date"/>
    </picture>
</template>