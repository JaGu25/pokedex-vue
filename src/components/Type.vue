<template>
    <span :class="[`bg-${color}-400`]" class="mx-1 text-sm px-2 py-1 text-white font-bold rounded-md">
        {{texto}}
    </span>
</template>

<script>
export default {
    props: {
        color: '',
        texto: ''
    }
}
</script>