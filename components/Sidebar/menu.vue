<template>
    <div>
        <header class="flex items-center gap-2 p-4 hover:scale-[101%] transition cursor-pointer">
            <span>{{ items.title }}</span>
        </header>
    </div>
</template>

<script setup>
const items = ref([
    {
        title: "a",
        path: "/login"
    },{
        title: "b",
        path: "/login"
    },{
        title: "c",
        path: "/login"

    },{
        title: "d",
        path: "/login"

    },{
        title: "e",
        path: "/login"

    },
])

</script>

<style  scoped>

</style>