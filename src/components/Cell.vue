<template>
    <div class='cell' @click='onClick'>
        <p>{{ sign }}</p>
    </div>
</template>

<script>
    export default {
        data() {
            return {};
        },

        props: {
            sign: String,
            row: Number,
            col: Number
        },

        methods: {
            onClick() {
                this.$emit('mark', this.row, this.col);
            }
        }
    };
</script>

<style>
    .cell {
        width: 50px;
        height: 50px;
        border: 1px solid #444;
        float: left;
        text-align: center;
    }

    .cell p {
        margin: auto;
        font-size: 40px;
    }
</style>