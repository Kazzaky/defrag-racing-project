<script setup>
    import { Head } from '@inertiajs/vue3';

    const props = defineProps({
        page: Object
    });

</script>

<template>
    <div>
        <Head :title="props.title" />

        <div class="py-8">
            <div class="max-w-8xl mx-auto sm:px-6 lg:px-8">
                <h2 class="font-semibold text-3xl text-gray-200 leading-tight mb-2">
                    {{ page.title }}
                </h2>
            </div>
        </div>

        <div class="py-4">
            <div class="max-w-8xl mx-auto sm:px-6 lg:px-8 text-gray-300 prose" v-html="page.content"></div>
        </div>
    </div>
</template>