<template>
    <div>
        <h1>首页</h1>
        <router-link to="/about">跳转到 about</router-link>

        <!--{{$store.state.count}}-->
        {{count}}

        <button @click="handleIncrement">+1</button>
        <button @click="handleDecrease">-1</button>

        <div>{{list}}</div>
        <div>{{listCount}}</div>

        <button @click="handleActionIncrement">action +1</button>
        <button @click="handleAsyncIncrement">action +1</button>

        <Counter :number="number"></Counter>
    </div>
</template>
<script>
    import Counter from './counter.vue';

    export default {
        components: {
            Counter
        },
        data() {
            return {
                number: 0
            }
        },
        created() {
            this.$bus.on('add', num=>{
                this.number += num;
            });
        },
        beforeDestroy() {
            // this.$bus.off('add', this.addR);
        },
        computed: {
            count() {
                return this.$store.state.count;
            },
            list() {
                //return this.$store.state.list.filter(item => item < 10);
                return this.$store.getters.filteredList;
            },
            listCount() {
                return this.$store.getters.listCount;
            }
        },
        methods: {
            handleIncrement() {
                this.$store.commit('increment');
            },
            handleDecrease() {
                this.$store.commit('decrease');
            },
            handleActionIncrement() {
                this.$store.dispatch('increment');
            },
            handleAsyncIncrement() {
                this.$store.dispatch('asyncIncrement').then(()=>{
                    console.log(this.$store.state.count);
                });
            }
        }
    }
</script>