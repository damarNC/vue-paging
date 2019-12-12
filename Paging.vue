<template>
    <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li :class="'page-item'"><button @click="btnFirst()" class="page-link">First</button></li>
            <li :class="'page-item ' + disablePrev">
                <button @click="!disablePrev ? btnPrev() : ''" class="page-link">
                    Prev
                </button>
            </li>

            <li :class="'page-item ' + n.activeClass" v-for="n in pages" :key="n.content">
                <button class="page-link" @click="emitBtnClick(n.content)">
                    {{ n.content }}
                </button>
            </li>

            <li :class="'page-item ' + disableNext">
                <button @click="!disableNext ? btnNext() : ''" class="page-link">
                    Next
                </button>
            </li>
            <li :class="'page-item'"><button @click="btnLast()" class="page-link">Last</button></li>
        </ul>
    </nav>
</template>

<script>
export default {
    name : 'Paging',
    props : {
        totalPage : {
            type : Number,
            required: true
        },
        currentPage : {
            type : Number,
            default: 1,
        },
        pageRange: {
            type: Number,
            default: 3
        }
    },
    data() {
        return {
            selected: this.currentPage,
            disableNext : '',
            disablePrev : '',
        }
    },
    mounted()
    {
        this.disablePrevNext();
    },
    computed: {
        pages : function()
        {
            let items = [];
            let range = this.pageRange * 2;

            if(this.totalPage<=range){
                range = this.totalPage - 1;
            }

            for (let i = 0; i <= range; i++) {
                let aktif = 'active primary';
                let cpage = this.currentPage;

                let limit;

                if((this.currentPage - this.pageRange) < 1){
                    limit = 0;
                } else {
                    if((this.currentPage + this.pageRange) >= this.totalPage){
                        limit = this.totalPage - (range + 1);
                    } else {
                        limit = (this.currentPage - 1) - range + this.pageRange;
                    }
                }

                if( ((i+1) + limit ) == cpage){
                    aktif = 'active primary';
                } else{
                    aktif = '';
                }
                let page = {
                    content: (i+1) +limit,
                    activeClass : aktif
                }

                items[i] = page;
            }

            return items;
        }
    },
    methods : {
        emitBtnClick : function(n)
        {
            this.selected = n;
            this.$emit('btnClick', n);
            this.disablePrevNext();
        },
        btnNext : function()
        {
            this.selected++;
            this.$emit('btnClick', this.selected);

            this.disablePrevNext();
        },
        btnPrev : function()
        {
            this.selected--;
            this.$emit('btnClick', this.selected);

            this.disablePrevNext();
        },
        btnFirst : function()
        {
            this.selected = 1;
            this.$emit('btnClick', this.selected);

            this.disablePrevNext();
        },
        btnLast : function()
        {
            this.selected = this.totalPage;
            this.$emit('btnClick', this.selected);

            this.disablePrevNext();
        },
        disablePrevNext : function()
        {
            this.disablePrev = this.selected == 1 ? 'disabled' : '';
            this.disableNext = this.selected == this.totalPage ? 'disabled' : '';
        }
    }
}
</script>