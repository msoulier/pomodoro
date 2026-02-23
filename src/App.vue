<script setup>
import { ref } from "vue";
import { invoke } from "@tauri-apps/api/core";

// minutes
let work_time = ref(25);
let rest_time = ref(5);

// seconds
let countdown = ref(0);

let work_count_job = null;
let rest_count_job = null;

function work_count() {
    countdown--;
    if (countdown <= 1) {
        alert("Stop working, get up and move around");
        rest();
    } else {
        work_count_job = setTimeout(function() { work_count(); }.bind(this), 1000);
    }
}

function rest_count() {
    countdown--;
    if (countdown <= 1) {
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
    countdown = rest_time * 60;
    rest_count_job = setTimeout(function() { rest_count(); }.bind(this), 1000);
}

function start() {
    if (work_count_job !== null) {
        clearTimeout(work_count_job);
        work_count_job = null;
    }
    countdown = work_time * 60;
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
    countdown = work_time * 60;
}

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
                            <select v-model="work_time">
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
                            <select v-model="rest_time">
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
                            <span class="px-4"></span>
                            <button type="button"
                                    @click="stop()"
                                    class="btn btn-outline-secondary">Stop</button>
                            <span class="px-4"></span>
                            <button type="button"
                                    @click="reset()"
                                    class="btn btn-outline-secondary">Reset</button>
                        </div>
                    </div>
                    <div class="row">
                        <div class="h-100 p-5 bg-body-tertiary border rounded-3">
                            <h2>Countdown</h2>
                                <p>{{ countdown }} seconds</p>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </main>
</template>
