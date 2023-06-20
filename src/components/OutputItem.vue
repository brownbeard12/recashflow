<script setup>
defineProps(['label'])

</script>

<template>
    <div>
        <p><label>{{ label }}</label></p>
        <p>
            <slot></slot>
        </p>
    </div>
</template>