<template>
    <div class="icon-set clearfix">

        <div class="input-group search">
            <input type="text" class="form-control" placeholder="Search icon..." @keyup="filterIcons" v-model="search">
            <span class="input-group-btn">
                <button class="btn btn-primary" :disabled="! search" type="button" @click="resetSearch">
                    <i class="mdi mdi-close"></i>
                </button>
            </span>

        </div>

        <ul class="icons">
            <li v-for="icon in result">
                <a href="#" @click="showDetail(icon)">
                    <i class="mdi" :class="'mdi-' + icon.name"></i>
                </a>
            </li>
        </ul>
    </div>
</template>

<script>
    import Icons from './mdi-icons.json'

    export default {
        data () {
            return {
                icons: Icons['icons'],
                result: null,
                filterSet: {},
                search: ''
            }
        },

        mounted() {
            this.result = this.icons;
        },

        methods: {
            showDetail: function (icon) {
                this.$emit('show-icon-detail', icon)
            },

            filterIcons: function () {

                let searchTerm = this.search.toLowerCase();

                this.result = this.icons.filter(icon => {

                    if (icon.name.toLowerCase().includes(searchTerm)) return icon;

                    if ( ! icon.aliases) return;

                    for (let i = 0; i < icon.aliases.length; i++) {
                        if( icon.aliases[i].includes(searchTerm) ) return icon;
                    }

                });

            },

            resetSearch: function () {
                this.result = this.icons;
                this.search = '';
            }
        }
    }
</script>

<style>

    .search {
        max-width: 400px;
        padding-bottom: 30px;
        margin: 0 15px;
    }

    .search .btn:disabled {
        background: #a4a4a4;
        border-color: #a4a4a4;
    }

    .search .btn:hover:not([disabled]) {
        cursor: pointer;
    }

    ul.icons {
        list-style: none;
        padding: 0;
        margin: 0;
    }

    ul.icons li {
        display: inline-block;
        float: left;
        padding: 15px;
        font-size: 30px;
    }

    ul.icons li a {
        color: #2d2d2d;
    }
</style>
