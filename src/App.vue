<script setup>
import { ref, computed, onMounted } from "vue";
import { invoke } from "@tauri-apps/api/core";

// minutes
let work_time = ref(25);
let rest_time = ref(5);

// seconds
let work_countdown = ref(0);
let rest_countdown = ref(0);

let work_count_job = null;
let rest_count_job = null;

let work_countdown_minutes = computed(() => {
    return parseInt(work_countdown.value / 60);
});

let work_countdown_seconds = computed(() => {
    return work_countdown.value - parseInt(work_countdown.value / 60)*60;
});

let rest_countdown_minutes = computed(() => {
    return parseInt(rest_countdown.value / 60);
});

let rest_countdown_seconds = computed(() => {
    return rest_countdown.value - parseInt(rest_countdown.value / 60)*60;
});

function toggle() {
    if (work_count_job !== null) {
        stop();
        rest();
    } else {
        stop();
        start();
    }
}

function work_count() {
    work_countdown.value--;
    if (work_countdown.value <= 1) {
        alert("Stop working, get up and move around");
        rest();
    } else {
        work_count_job = setTimeout(function() { work_count(); }.bind(this), 1000);
    }
}

function rest_count() {
    rest_countdown.value--;
    if (rest_countdown.value <= 1) {
        alert("Start working, rest time is over");
        start();
    } else {
        rest_count_job = setTimeout(function() { rest_count(); }.bind(this), 1000);
    }
}

function rest() {
    if (rest_count_job !== null) {
        clearTimeout(rest_count_job);
        rest_count_job = null;
    }
    rest_count_job = setTimeout(function() { rest_count(); }.bind(this), 1000);
}

function start() {
    if (work_count_job !== null) {
        clearTimeout(work_count_job);
        work_count_job = null;
    }
    work_count_job = setTimeout(function() { work_count(); }.bind(this), 1000);
}

function stop() {
    if (work_count_job !== null) {
        clearTimeout(work_count_job);
        work_count_job = null;
    }
    if (rest_count_job !== null) {
        clearTimeout(rest_count_job);
        rest_count_job = null;
    }
}

function reset() {
    work_countdown.value = work_time.value * 60;
    rest_countdown.value = rest_time.value * 60;
}

function init() {
    reset();
}

onMounted(() => {
    reset();
});

</script>

<template>
    <main class="container">
        <div class="row">
            <h1>Pomodoro: Get off your ass regularly</h1>
        </div>
        <div class="row">&nbsp;</div>
        <div class="row">
            <div class="container-fluid">
                <form>

                    <div class="row">
                        <div class="col-6 bg-body-tertiary border rounded-3">
                            Time between breaks
                        </div>
                        <div class="col-6">
                            <select v-model="work_time" @change="reset()">
                                <option value="5">5 min</option>
                                <option value="10">10 min</option>
                                <option value="15">15 min</option>
                                <option value="20">20 min</option>
                                <option value="25">25 min</option>
                                <option value="30">30 min</option>
                            </select>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-6 bg-body-tertiary border rounded-3">
                            Break time
                        </div>
                        <div class="col-6">
                            <select v-model="rest_time" @change="reset()">
                                <option value="5">5 min</option>
                                <option value="10">10 min</option>
                                <option value="15">15 min</option>
                                <option value="20">20 min</option>
                                <option value="25">25 min</option>
                                <option value="30">30 min</option>
                            </select>
                        </div>
                    </div>
                    <div class="row p-4">
                        <div class="col-6"></div>
                        <div class="col-6">
                            <button type="button"
                                    @click="start()"
                                    class="btn btn-outline-secondary">Start</button>
                            <span class="px-2"></span>
                            <button type="button"
                                    @click="stop()"
                                    class="btn btn-outline-secondary">Stop</button>
                            <span class="px-2"></span>
                            <button type="button"
                                    @click="reset()"
                                    class="btn btn-outline-secondary">Reset</button>
                            <span class="px-2"></span>
                            <button type="button"
                                    @click="toggle()"
                                    class="btn btn-outline-secondary">Toggle</button>
                        </div>
                    </div>
                    <div class="row">
                        <div class="h-100 p-5 bg-body-tertiary border rounded-3">
                            <h2>Countdown</h2>
                            <div class="row">
                                <p>Work: {{ work_countdown_minutes }} minutes, {{ work_countdown_seconds }} seconds remaining <span class="badge text-bg-secondary" v-if="work_count_job !== null">Running</span></p>
                            </div>
                            <div class="row">
                                <p>Rest: {{ rest_countdown_minutes }} minutes, {{ rest_countdown_seconds }} seconds remaining <span class="badge text-bg-secondary" v-if="rest_count_job !== null">Running</span></p>
                            </div>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </main>
</template>
