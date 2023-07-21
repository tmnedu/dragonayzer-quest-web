<!--
    Сделать звуки
    Сделать тосты
-->
<script setup>
import { ref, onMounted } from "vue";
import testImg from "@/assets/test.jpg";
import * as bootstrap from "bootstrap";
const t1 =
    "1234? И Это Пароль??? рух. 14?Это Пароль? Бр 1234?  Это Проль? 4? ИЭто ароль??? БарольБрух. 1234?Этоуто Пароль??? Брух. Брух. 14? Ио Паро?? Бх о Пароль??? Бр? И Это Пароль??";
const t2 = "Rrrrrred";
let displayedText = ref("Привет.");
function setText(text) {
    displayedText.value = text;
}
const badges = ref([
    {
        pw: "01BRYGPW",
        unlocked: false,
        class: "color-indicator color-green",
        badgeText: "Слабый пасс",
        hidden: false,
        img: testImg,
        action: () => setText(t1),
    },
    {
        pw: "RRRRRRRR",
        unlocked: false,
        class: "color-indicator color-red",
        badgeText: "Биткойн",
        hidden: false,
        img: testImg,
        action: () => setText(t2),
    },
    {
        pw: "01010101",
        unlocked: false,
        class: null,
        badgeText: null,
        hidden: true,
        img: null,
        action: () => window.open("https://ya.ru", "_blank"),
    },
    {
        pw: "WPGYRB10",
        unlocked: false,
        class: null,
        badgeText: null,
        hidden: true,
        img: null,
        action: () => (catMode.value = true),
    },
]);
const btns = [
    { class: "color-black", key: "0" },
    { class: "color-white", key: "1" },
    { class: "color-blue", key: "B" },
    { class: "color-red", key: "R" },
    { class: "color-yellow", key: "Y" },
    { class: "color-green", key: "G" },
    { class: "color-purple", key: "P" },
    { class: "color-cyan", key: "W" },
];

let indicators = ref([
    { class: "color-red", key: "R" },
    { class: "color-red", key: "R" },
    { class: "color-red", key: "R" },
    { class: "color-red", key: "R" },
    { class: "color-red", key: "R" },
    { class: "color-red", key: "R" },
    { class: "color-red", key: "R" },
    { class: null, key: null },
]);

let counter = ref(0);

let catMode = ref(false);

const storageKey = "Koishi's Personal Storage Of TERRIBLE Things";

function saveBadge(badge) {
    let savedBadges = [];
    let storedString = localStorage.getItem(storageKey);
    if (storedString) savedBadges = JSON.parse(storedString);
    if (!savedBadges) savedBadges = [];
    if (!savedBadges.includes(badge.pw)) savedBadges.push(badge.pw);
    localStorage.setItem(storageKey, JSON.stringify(savedBadges));
}
let buffer = [];
// function executeBuffer() {
//     buffer.forEach((btn) => enter(btn));
//     buffer = [];
// }
// function bufferInput(btn) {
//     if (buffer.length < 8) buffer.push(btn);
// }
function enter(btn) {
    let nextIndicator = indicators.value.find((x) => !x.key);
    if (passwordSetByBadge.value) {
        clearPassword();
        nextIndicator = indicators.value[0];
        passwordSetByBadge.value = false;
    }
    if (!nextIndicator) return; //bufferInput(btn); // nextIndicator = indicators.value[0];
    nextIndicator.key = btn.key;
    nextIndicator.class = btn.class;
    if (!indicators.value.some((x) => !x.key)) checkPassword();
}
function checkPassword() {
    const resultPassword = indicators.value.map((x) => x.key).join("");
    const badge = badges.value.find((badge) => badge.pw == resultPassword);
    setTimeout(clearPassword, 500);
    if (!badge) return;
    saveBadge(badge);
    badge.unlocked = true;
    badge.action();
}
function clearPassword() {
    indicators.value = indicators.value.map((x) => ({
        class: null,
        key: null,
    }));
    // executeBuffer();
}
let passwordSetByBadge = ref(false);

function activateBadge(badge) {
    badge.action();
    indicators.value.forEach((indicator, i) => {
        const key = badge.pw[i];
        indicator.key = key;
        indicator.class = btns.find((btn) => btn.key == key).class;
    });
    passwordSetByBadge.value = true;
}
let toastLiveExample = null;
let toastBootstrap = null;
function surprise() {
    toastLiveExample = document.getElementById("liveToast");
    toastBootstrap = bootstrap.Toast.getOrCreateInstance(toastLiveExample);
    const surprise = document.getElementById("surprise");
    toastBootstrap.show();
    surprise.play();
    if (toastText.value == "You know the rules,") {
        toastText.value = "Опять?"
        toastSubText.value = "Чел, ты..."
    }
}
let toastText = ref("Рекламная пауза");
let toastSubText = ref("С любовью");
function stopSurprise() {
    toastLiveExample = document.getElementById("liveToast");
    toastBootstrap = bootstrap.Toast.getOrCreateInstance(toastLiveExample);
    const surprise = document.getElementById("surprise");
    surprise.pause();
    setTimeout(() => {
        toastText.value = "You know the rules,"
        toastSubText.value = "and so do I..."
        toastBootstrap.show();
    }, 2000);
    setTimeout(() => {
        surprise.currentTime = 23;
        surprise.playbackRate += 2.0;
        surprise.play();
    }, 3000);
    setTimeout(() => {
        surprise.pause();
        toastBootstrap.hide();
    }, 4300);
}
onMounted(() => {
    console.error("Внимание!");
    console.error("   Тревога!");
    console.warn(
        "Происходит взлом жопы! Пожалуйста не нужно ковыряться в моём коде... UwU"
    );
    let savedBadges = null;
    let storedString = localStorage.getItem(storageKey);
    if (storedString) savedBadges = JSON.parse(storedString);
    if (!savedBadges) return;
    badges.value.forEach((badge) => {
        if (savedBadges.includes(badge.pw)) badge.unlocked = true;
    });
});
</script>

<template>
    <div class="app">
        <Transition mode="out-in">
            <div
                class="d-flex justify-content-center mt-3 px-3 pb-3 text-center manager-of-underwater-creatures"
                v-text="displayedText"
                :key="displayedText"
            />
        </Transition>
        <div class="d-flex gap-2 pb-0 justify-content-center flex-wrap">
            <div
                class="color-indicator"
                :class="indicator?.class"
                v-for="indicator in indicators"
            />
        </div>
        <div
            class="d-flex gap-2 justify-content-center flex-wrap tigrinaya-zhopa"
        >
            <div v-for="btn in btns" :class="{ catMode }">
                <button
                    class="btn rounded-circle p-4 lh-1"
                    :class="btn.class"
                    @click="enter(btn)"
                />
            </div>
        </div>
        <div
            class="d-flex px-2 pb-2 gap-2 align-items-center justify-content-center flex-wrap"
            style="margin-top: auto"
        >
            <template v-for="i in 6">
                <span
                    v-for="badge in badges.filter(
                        (x) => x.unlocked && !x.hidden
                    )"
                    class="badge d-flex align-items-center p-1 pe-2 text-primary-emphasis bg-primary-subtle border border-primary-subtle rounded-pill"
                    :class="badge.class"
                    @click="activateBadge(badge)"
                >
                    <img
                        class="rounded-circle me-1"
                        :src="badge.img"
                        width="24"
                        height="24"
                    />
                    <span v-text="badge.badgeText" />
                </span>
            </template>
        </div>
        <!-- <button @click="surprise()">surprise</button> -->
        <div class="toast-container position-fixed bottom-0 end-0 p-3">
            <div
                id="liveToast"
                class="toast fade hide"
                role="alert"
                aria-live="assertive"
                aria-atomic="true"
            >
                <div class="toast-header">
                    <svg
                        class="bd-placeholder-img rounded me-2"
                        width="20"
                        height="20"
                        xmlns="http://www.w3.org/2000/svg"
                        aria-hidden="true"
                        preserveAspectRatio="xMidYMid slice"
                        focusable="false"
                    >
                        <rect width="100%" height="100%" fill="#007aff" />
                    </svg>
                    <strong
                        class="me-auto"
                        style="line-height: 1"
                        v-text="toastText"
                    />
                    <small v-text="toastSubText" />
                    <button
                        v-if="toastSubText != 'and so do I...'"
                        type="button"
                        class="btn-close"
                        id="stopITPLEASESTOPIT"
                        data-bs-dismiss="toast"
                        aria-label="Close"
                        @click="stopSurprise()"
                    />
                </div>
                <div class="toast-body">
                    <video
                        id="surprise"
                        src="/surprise.mp4"
                        style="height: auto; width: 100%"
                    />
                </div>
            </div>
        </div>
    </div>
</template>
