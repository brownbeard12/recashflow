<script setup>
defineProps(['label', 'numSteps', 'modelValue'])
defineEmits(['update:modelValue'])

</script>

<template>
    <div>
        <p><label>{{ label }}</label></p>
        <p><input type="number" :step="numSteps" :value="modelValue"
                @input="$emit('update:modelValue', $event.target.value)" /></p>
        <slot></slot>
    </div>
</template>