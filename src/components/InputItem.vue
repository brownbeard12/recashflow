<script setup>
defineProps(['label', 'numSteps', 'modelValue'])
defineEmits(['update:modelValue'])

</script>

<template>
    <div>
        <p><label>{{ label }}</label></p>
        <p><input type="number" min=0 :step="numSteps" :value="modelValue"
                @input="$emit('update:modelValue', $event.target.value)" />
            <slot></slot>
        </p>
    </div>
</template>