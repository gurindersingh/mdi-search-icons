<template>
    <div id="app">

        <navbar></navbar>

        <modal :icon="icon" :show="!! icon" @close-modal="closeModal" @close-icon-modal="closeModal"></modal>

        <div class="container-fluid">
            
            <div class="row">
                
                <div class="col-lg-12">
                    
                    <icons @show-icon-detail="showDetailInModal"></icons>
                    
                </div>

            </div>

        </div>

        <button class="top btn btn-primary" @click="goToTop">Top</button>

    </div>
</template>

<script>
    import Navbar from './components/navbar'
    import Icons from './components/Icons'
    import Modal from './components/modal'

    export default {
        name: 'app',
        components: {
            Navbar,
            Icons,
            Modal
        },
        data() {
            return {
                icon: null
            }
        },
        
        methods: {
            showDetailInModal: function (icon) {
                this.icon = icon;
            },
            closeModal: function () {
                this.icon = null;
            },
            goToTop: function () {
                window.scrollTo(0, 0);
            }
        }
    }
</script>

<style>
    body {
        padding: 120px 0;
    }

    .top {
        position: fixed;
        right: 20px;
        bottom: 20px;
        opacity: 0.4;
        border-radius: 50%;
        padding: 15px 15px;
    }

    .top:hover {
        opacity: 1;
        cursor: pointer;
    }
</style>
