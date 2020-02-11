<template>
    <div v-if="storage.length > 0" >
        <h2>History of move</h2>
        <ul>
            <li v-for="(item, index) in storage" :key='index' >
            ( {{ item.x }}, {{ item.y }} ) : {{ item.sign }} 
            <button @click='onClick(index)'>to</button>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {};
        },

        props: {
           storage: Array
        },

        methods: {
            onClick(index) {
                this.$emit('return', index);
            }
        }
    }
</script>