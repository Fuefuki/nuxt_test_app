<script setup lang="ts">
const route = useRoute();
const id = String(route.params.id);
const { data: article } = await useFetch(`/api/reviewDetail`, {
    params: { id: id },
});
if (!article.value) {
    throw createError({ statusCode: 404, statusMessage: 'Page Not Found' })
}

const deleteReview = async () => {
    await useFetch('/api/reviewDelete', {
        method: 'delete',
        body: { 
            id: id,
        }
    })
    await navigateTo('/lists/')
}
</script>

<template>
    <div class="flex flex-col">
        <h1 class="title text-2xl font-bold">{{ article.title }}</h1>
        <div class="p-3 md">{{ article.body }}</div>
        <nuxt-link class="button-blue" :to="`${id}/edit`">更新</nuxt-link>
        <button @click="deleteReview" class="button-red">削除</button>
    </div>
</template>