<template>
    <div class="game">
        <div class="main">
            <div class="field" >
                <field :field='field' :size='size' @mark="onClick" />
            </div>

            <div class="clear"></div>

            <div v-if='finish' class="game-result-message">
                <p>{{ resultMessage }}</p>
            </div>
            
            <div class="new-game">
                <button @click='clearGameData'>New game</button>
            </div>
        </div>

        <div class="history">
            <history :storage='storage' @return='backToMove' />
        </div>
    </div>
</template>

<script>
    import Field from './Field';
    import History from './History';

    export default {
        
        data () {
            return {
                size: 4,
                field: null,
                move: {},
                clearHistory: false,
                countMoving: 0,
                win: false,
                finish: false,
                storage: []
            };
        },


        components: {
            Field,
            History
        },

        created() {
            this.clearGameData();
        },

        computed: {
            sign() {
                return this.storage.length % 2 === 0 ? 'o' : 'x'; 
            },

            resultMessage() {
                if (this.win) {
                    return `This game is won by '${this.sign.toUpperCase()}'!`; 
                }

                if (this.finish) {
                    return 'Friendship won';
                }
            }
        },

        methods: {
            onClick(row, col) {
                let y = col - 1;
                let x = row - 1;

                if (this.field[x][y] === undefined && !this.finish && !this.win) {
                    this.clearHistory = false;
                    this.$set(this.field[x], y, this.sign);
                    this.move = {
                        x,
                        y,
                        sign: this.sign
                    };
                    this.win = this.checkWinner(x, y, this.sign);
                    if (!this.win) {
                        this.storage.push(this.move);
                    }
                   
                    this.countMoving++;

                    if (this.countMoving >= this.size * this.size || this.win) {
                        this.finish = true;
                    }    
                }
            },

            checkWinner(x, y, sign) {
                let horizontal = 0, 
                    vertical = 0, 
                    diagonal = 0, 
                    reverseDiagonal = 0;

                for (let i = 0; i < this.size; i++ ) {
                    horizontal += this.field[x][i] === sign ? 1 : 0;
                    vertical += this.field[i][y] === sign ? 1 : 0;
                    diagonal += this.field[i][i] === sign ? 1 : 0;
                    reverseDiagonal += this.field[i][this.size - (i + 1)] === sign ? 1 : 0;
                }

                return horizontal === this.size
                    || vertical === this.size
                    || diagonal === this.size
                    || reverseDiagonal === this.size;

            },

            backToMove(index) {
                if (this.finish) {
                    return;
                }

                this.countMoving = index + 1;
                this.storage.splice(this.countMoving);
                let temp = this.getEmptyField(this.size);

                this.storage.forEach(move => {
                    temp[move.x][move.y] = move.sign;
                });
                
                this.field = temp;
            },

            getEmptyField(length) {
                return new Array(length).fill('').map(() => new Array(length));
            },

            clearGameData() {
                this.field = this.getEmptyField(this.size);
                this.clearHistory = true;
                this.countMoving = 0;
                this.finish = false;
                this.win = false;
                this.storage = [];
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
        display: flex;
    }

    .history {
        margin-left: 20px;
    }
</style>