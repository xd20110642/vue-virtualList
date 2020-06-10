<template>
    <div @scroll="handleScrollEvent" class="mainBox">
        <div :style="'height:'+listHeight+'px'" class="mainBox-phantom"></div>
        <div :style="'transform:'+getTransform" class="mainBox-content">
            <div :key="item.id"
                 :style="'height:'+itemSize+'px'"
                 class="mainBox-item"
                 v-for="(item,index) of visibleData"
            >
                {{item.id}}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Virtuallist",
        props: {
            // 数据量
            data: {
                type: Array,
                default: () => []
            },
            //每项高度
            itemSize: {
                type: Number,
                default: 100
            },
            //父盒子可视区域高度
            screenHeight: {
                type: Number,
                default: 1000
            }
        },
        data() {
            return {
                //偏移量
                startOffset: 0,
                //起始索引
                start: 0,
                //结束索引
                end: null,
            }
        },
        computed: {
            //列表总高度
            listHeight() {
                return this.data.length * this.itemSize;
            },
            //可显示的列表项数
            visibleCount() {
                return Math.ceil(this.screenHeight / this.itemSize)
            },
            //偏移量对应的style
            getTransform() {
                return `translateY(${this.startOffset}px)`;
            },
            //获取真实显示列表数据 截取数据 用截取的尾部索引和最小值来
            visibleData() {
                return this.data.slice(this.start, Math.min(this.end, this.data.length));
            }
        },
        methods: {
            handleScrollEvent(event) {
                //获取到滚动距离
                let scrollTop = event.target.scrollTop;
                //此时的开始索引
                this.start = Math.floor(scrollTop / this.itemSize);
                //此时的结束索引
                this.end = this.start + this.visibleCount;
                //此时的偏移量
                this.startOffset = scrollTop - (scrollTop % this.itemSize);
            }
        },
        mounted() {
            this.start = 0;
            this.end = this.start + this.visibleCount;
        }
    }
</script>

<style>
    .mainBox {
        height: 100%;
        position: relative;
        overflow: auto;
    }

    .mainBox-phantom {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        z-index: -1;
    }

    .mainBox-content {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        text-align: center;
    }

    .mainBox-item {
        color: #555;
        box-sizing: border-box;
        border-bottom: 1px solid #999;
    }
</style>