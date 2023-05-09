<template>
    <select @change="changeOption" style="border-radius: 12px;">
        <option disabled selected value="">Choose from an option list</option>
        <option 
        v-for="option in options" 
        :key="option.value"
        :value="option.value"
        >
            {{option.name}}
        </option>
    </select>
</template>

<script>
export default {
    name: "MySelect",
    props: {
        modelValue: {
            type: String
        },
        options: {
            type: Array,
            default: () => []
        }
    },
    methods: {
        changeOption(event) {
            this.$emit("update:modelValue", event.target.value);
        }
    }
}
</script>

<style scoped>
    
</style>