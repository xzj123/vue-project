<template>
    <transition name="modal">
        <div v-show="show">
            <div class="modal" @click.self="clickBackdrop">
                <slot name="modal-dialog"></slot>
            </div>
            <div class="modal-backdrop in"></div>
        </div>
    </transition>
</template>

<script>
    export default {
        props: {
            show: {
                type: Boolean,
                default: false
            },
        },
        watch: {
            show (value) {
                if (value) {
                    document.body.className += ' modal-open';
                }
            }
        },
        created () {
            if (this.show) {
                document.body.className += ' modal-open';
            }
        },
        beforeDestroy () {
            document.body.className = document.body.className.replace(/\s?modal-open/, '');
        },
    };
</script>
<style scoped>
    .modal {
        display: block;
    }
    .modal-enter-active, .modal-leave-active {
        transition: opacity .2s;
    }
    .modal-enter, .modal-leave-to {
        opacity: 0;
    }
    .modal-backdrop {
        opacity: 0.3;
    }
</style>