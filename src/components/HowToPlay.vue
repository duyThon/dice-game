<template>
    <div class="wrapper-popup" v-bind:class='getClassPopup'>
        <div class="rule">
            <h3>How To Play</h3>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.
                Vel aut voluptas laboriosam, quam distinctio quae mollitia iste quis sunt libero
                omnis incidunt magni repellat, at fugit sed dolorum. Hic, non.</p>
            <button
                v-on:click='confirmAndStart' 
                class="btn">Done</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'how-to-play',
    data() {
        return {

        }
    },
    props: {
        isOpenPopup: {
            type: Boolean,
            default: false
        }
    },
    computed: {
        getClassPopup() {
            return {
                'open-popup': this.isOpenPopup
            }
        }
    },
    methods: {
        confirmAndStart() {
            this.$emit('handleConfirmEvent');
        },
    }
}
</script>

<style lang="postcss" scoped>
    .wrapper-popup {
        opacity: 0;
        visibility: hidden;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.541);
        transition: all .3 ease;
        z-index: 99;
    }

    .wrapper-popup.open-popup {
        opacity: 1;
        visibility: visible;
    }
    
    .rule {
        position: absolute;
        background-color: #fff;
        width: 400px;
        height: 300px;
        top:50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
        padding: 20px;
        font-size: 17px;
        transition: all .3 ease;
    }
    
    .open-popup .rule {
        transform: translate(-50%, -50%) scale(1.2);
    }

    .rule h3 {
        margin-bottom: 20px;
        font-size: 30px;
        text-align: center;
    }

    .btn {
        background-color: #fff;
        border: 2px solid #333;
        padding: 10px 15px;
        position: absolute;
        bottom: 15px;
        right: 15px;
    }

    .btn:hover {
        cursor: pointer;
        background-color: #333;
        color: #fff;
    }
    
</style>