<script setup>
import { storeToRefs } from 'pinia';

import { useAuthStore } from '@/stores';

const authStore = useAuthStore();
const { user } = storeToRefs(authStore);
</script>
    
<template>
    <div>
        <div  v-if="user">
            <h1>Hi {{user.username}}!</h1>
            <p>
                <router-link to="/users">Manage Users</router-link>
            </p>
        </div>

        <div id="coffeeContainer">
            <div class="steam" id="steam1"> </div>
            <div class="steam" id="steam2"> </div>
            <div class="steam" id="steam3"> </div>
            <div class="steam" id="steam4"> </div>

            <div id="cup">
                <div id="cup-body"></div>
                <div id="cup-handle"></div>
                <div id="eye-left"></div>
                <div id="eye-right"></div>
                <div id="smile"></div>
            </div>

            <div id="saucer"></div>
        </div>
    </div>
</template>

<style scoped>
#header{
    background: #8acdeb;
}

#coffeeContainer {
    background: #8acdeb;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.steam {
    position: absolute;
    height: 150px;
    width: 150px;
    border-radius: 50%;
    background-color: #fff;
    margin-top: -75px;
    margin-left: 75px;
    z-index: 0;
    opacity: 0;
}

#steam1 {
    -webkit-animation: steam1 4s ease-out infinite;
    animation: steam1 4s ease-out infinite;
}

#steam3 {
    -webkit-animation: steam1 4s ease-out 1s infinite;
    animation: steam1 4s ease-out 1s infinite;
}

#steam2 {
    -webkit-animation: steam2 4s ease-out 0.5s infinite;
    animation: steam2 4s ease-out 0.5s infinite;
}

#steam4 {
    -webkit-animation: steam2 4s ease-out 1.5s infinite;
    animation: steam2 4s ease-out 1.5s infinite;
}

@-webkit-keyframes steam1 {
    0% {
        transform: translateY(0) translateX(0) scale(0.25);
        opacity: 0.2;
    }

    100% {
        transform: translateY(-200px) translateX(-20px) scale(1);
        opacity: 0;
    }
}

@keyframes steam1 {
    0% {
        transform: translateY(0) translateX(0) scale(0.25);
        opacity: 0.2;
    }

    100% {
        transform: translateY(-200px) translateX(-20px) scale(1);
        opacity: 0;
    }
}

@-webkit-keyframes steam2 {
    0% {
        transform: translateY(0) translateX(0) scale(0.25);
        opacity: 0.2;
    }

    100% {
        transform: translateY(-200px) translateX(20px) scale(1);
        opacity: 0;
    }
}

@keyframes steam2 {
    0% {
        transform: translateY(0) translateX(0) scale(0.25);
        opacity: 0.2;
    }

    100% {
        transform: translateY(-200px) translateX(20px) scale(1);
        opacity: 0;
    }
}

#cup {
    z-index: 1;
}

#cup-body {
    position: absolute;
    height: 300px;
    width: 250px;
    border-radius: 4px 4px 20px 20px;
    background-color: #fff;
    margin: auto;
    display: inline-block;
    overflow: hidden;
    z-index: 1;
}

#cup-handle {
    position: relative;
    height: 150px;
    width: 80px;
    border-radius: 160px 75px 150px 75px;
    border: 18px solid #F3F3F3;
    margin-top: 50px;
    margin-bottom: 0px;
    margin-left: 220px;
    display: inline-block;
    z-index: 0;
}

#saucer {
    position: absolute;
    height: 15px;
    width: 300px;
    border-radius: 0 0 150px 150px;
    background-color: #8a8888;
    border-color: #868585;
    border-width: 2px 2px;
    border-style: solid;
    margin-top: 95px;
    margin-left: -25px;
    z-index: 2;
}

#shadow {
    height: 10px;
    width: 300px;
    border-radius: 50%;
    margin-top: -5px;
    margin-left: 6px;
    background-color: #122436;
}

#smile {
    position: absolute;
    height: 30px;
    width: 55px;
    border-radius: 0px 0px 20px 20px;
    background-color: rgb(238, 0, 0);
    border-color: #000000;
    /* border-width: 18px 18px; */
    border-style: solid;
    margin-left: 100px;
    margin-top: -50px;
    /* display: inline-block; */
    overflow: hidden;
    z-index: 1;
}

#eye-left {
    position: absolute;
    height: 30px;
    width: 30px;
    border-radius: 20px 20px 20px 20px;
    background-color: rgb(0, 0, 0);
    margin-left: 60px;
    margin-top: -100px;
    /* display: inline-block; */
    overflow: hidden;
    z-index: 1;
}

@keyframes wink {
    0% {
        height: 30px;
        margin-top: -100px;
    }

    25% {
        height: 2px;
        margin-top: -86px;
    }

    50%,
    100% {
        height: 30px;
        margin-top: -100px;
    }
}

#eye-right {
    position: absolute;
    height: 30px;
    width: 30px;
    border-radius: 20px 20px 20px 20px;
    background-color: rgb(0, 0, 0);
    margin-left: 170px;
    margin-top: -100px;
    /* display: inline-block; */
    overflow: hidden;
    z-index: 1;

    /* animation: wink 3s 3s 3s infinite reverse ; */
    animation-name: wink;
    animation-duration: 5s;
    animation-iteration-count: infinite;
    /* animation: name duration timing-function delay iteration-count direction fill-mode; */
}
</style>