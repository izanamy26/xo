<template>
    <div>
        <div v-for="i in size" class='row' :key='i'>
            <cell v-for="n in size" :key='n' :sign='field[i - 1][n - 1]' :row='i' :col='n' @mark='onMark' />
        </div>
    </div>    
</template>

<script>
    import Cell from './Cell';

    export default {
        data () {
            return {};
        },

        props: {
            size: Number,
            field: Array
        },

        components: {
            Cell
        },

        methods: {
            onMark(row, col) {
                this.$emit('mark', row, col);
            }
        }

    };
</script>

<style>
    .row,
    .clear {
        clear: both;
    }

    .new-game {
        margin: 20px 0;
    }

    .game {
        margin: 30px;
    }
</style>