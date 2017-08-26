<template>
    <div class="overlay" v-if="show">
        <div class="modal" :style="{display: show ? 'block' : 'none'}">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" @click="closeModal"><span>&times;</span></button>
                        <h4 class="modal-title" v-text="icon ? icon.name : ''"></h4>
                    </div>
                    <div class="modal-body clearfix">

                        <div class="icon icon-xl">
                            <i class="mdi" :class="icon ? 'mdi-' + icon.name : ''"></i>
                        </div>

                        <div class="code">
                            <code>mdi mdi-{{ icon ? icon.name : '' }}</code>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-warning" @click="closeModal">Close</button>
                        <button type="button" class="btn btn-success" @click="downloadSVG">Download .svg</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import FileSaver from 'file-saver'

    export default {
        props: {
            icon: {
                type: Object
            },
            show: {
                type: Boolean,
                default: false
            }
        },
        data() {
            return {
                svg : null
            }
        },
        methods: {
            closeModal: function () {
                this.$emit('close-icon-modal');
            },
            downloadSVG: function () {
                let file = new Blob( [this.svg], {type:"application/svg+xml"} );

                FileSaver.saveAs(file, 'mdi-' + this.icon.name + '.svg');
            }
        },
        
        watch: {
            icon: function (icon) {
                if ( ! icon ) return;

                axios.get('/static/assets/svg/' + icon.name + '.svg')
                    .then( res => {
                        this.svg = res.data;
                    })
                    .catch( err => console.log(err) );
            }
        }
    }
</script>

<style>
    .overlay {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0,0,0,0.7);
        z-index: 100;
        overflow-y: auto;
    }

    .icon {
        float: left;
        display: inline-block;
        font-size: 50px;
        line-height: 1;
    }

    .code code {
        margin-left: 10px;
        /*padding: 20px;*/
        line-height: 50px;
    }

    .modal {
        /*position: relative;*/
        margin-top: 120px;
        z-index: 101;
    }
</style>