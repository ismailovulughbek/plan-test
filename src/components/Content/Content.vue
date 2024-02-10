<template>
    <div class="content">
        <div class="content-add">
            <div class="add">
                <input type="text" v-model="text" class="content-add__input" placeholder="Yangi vazifa qo'shish">
                <button class="content-add__btn" @click="addTask">
                    <img src="@/assets/imgs/plus.svg" alt="">
                </button>
            </div>
            <div class="time">
                <span>Bugun:</span>
                {{ nowTime.day }}.{{ nowTime.month }}.{{ nowTime.year }}
                ,
                {{ nowTime.hourse }}:{{ nowTime.minutes }}
            </div>
        </div>
        <div class="content-item">
            <ul class="content-item__list">
                <li class="content-item__list-li tudey" v-if="today">
                    <span class="day">
                        Bugun
                    </span>
                    <ul class="task-list">
                        <li v-for="item in todayList" :key="item">
                            <div class="task-list_check">
                                <input type="checkbox" v-model="key">
                                <span v-if="!key">
                                    {{ item }}
                                </span>
                                <span v-else class="no-active">
                                    <s>
                                        {{ item }}
                                    </s>
                                </span>
                            </div>
                            <span>
                                {{ parseInt(item.split(' ')[item.split(' ').length - 1]) ? item.split(' ')[item.split('').length - 1] : nowTime.hourse+1+':00'}}
                            </span>
                        </li>
                    </ul>
                </li>
                <li class="content-item__list-li tomorrow" v-if="tom">
                    <span class="day">
                        Ertaga
                    </span>
                    <ul class="task-list">
                        <li v-for="item in tomorList" :key="item">
                            <div class="task-list_check">
                                <input type="checkbox">
                                {{ item }}
                                <span>
                                </span>
                            </div>
                            <span>
                                {{ fullHours.test(item.split(' ')[item.split(' ').length - 1]) ? item.split(' ')[item.split(' ').length - 1] : " 19:00 " }} 
                            </span>
                        </li>
                    </ul>
                </li>
                <li class="content-item__list-li after" v-if="after">
                    <span class="day">
                        Keyin
                    </span>
                    <ul class="task-list">
                        <li v-for="item in afterList" :key="item">
                            <div class="task-list_check">
                                <input type="checkbox">
                                <span>
                                    {{ item }}
                                </span>
                            </div>
                            <span>
                                {{ fullDate.test(item.split(' ')[item.split(' ').length - 1]) ? item.split(' ')[item.split('').length - 1] : item.split(' ')[item.split(' ').length - 2] + "," + item.split('')[item.split(' ').length - 1] }}
                            </span>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { v4 as uuidv4 } from "uuid";
import { reactive, ref, onMounted } from "vue";
let text = ref('')


const date = new Date()
const nowTime = reactive({
    year: date.getFullYear(),
    month: date.getMonth() + 1 < 10 ? '0' + date.getMonth() + 1 : date.getMonth() + 1,
    day: date.getDate() < 10 ? '0' + date.getDate() : date.getDate(),
    hourse: date.getHours(),
    minutes: date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()
})


// TASK LIST 
let todayList = reactive([])
let tomorList = reactive([])
// let tomorrowOb = reactive({
//     id: uuidv4(),
//     title: '',
//     time: ''
// })
let afterList = reactive([])

// CHECKBOX's index
let todCheck = []
let tomCheck = []
let aftCheck = []

let feature = ref(false)

// GLOBAL DATE regEX
const fullHours = /^(?:[01]\d|2[0-3]):[0-5]\d$/;
const fullDate = /^(0[1-9]|[12]\d|3[01])\.(0[1-9]|1[0-2])\.\d{4}$/


// TASK SHOW FALSE or TRUE
let tom = ref(false)
let today = ref(false)
let after = ref(false)


const addTask = () => {
    let newA = text.value.split(" ")
    let len = newA.length

    // TOMORROW TEST in IF ELSE
    if (newA[len - 1].toLowerCase() == "ertaga" || newA[0].toLowerCase() == "ertaga") {
        if (parseInt(newA[len - 1]) && fullHours.test(newA[len - 1])) {
            tom.value = true
            // tomorrowOb.title = (newA.filter(e => e != "ertaga" && !fullHours.test(e))).join(" ")
            // tomorrowOb.time = (newA.filter(e => fullHours.test(e))).join(" ")
            tomorList.push(text.value)
            console.log(tomorList)

        } else if (newA[len - 1].toLowerCase() == "ertaga" || newA[0].toLowerCase() == "ertaga") {
            tom.value = true
            // tomorrowOb.title = (newA.filter(e => e != "ertaga")).join(" ")
            // tomorrowOb.time = "19:00"
            tomorList.push(text.value)
            console.log("ishladi 2");
        }


        // AFTER TEST in IF ELSE
    } else if ((parseInt(newA[len - 1]) || parseInt(newA[0])) && (fullDate.test(newA[len - 2]) || fullDate.test(newA[len - 1]) || fullDate.test(newA[0]))) {
        after.value = true
        afterList.push(text.value)
        console.log("ishladi AFTER");
        console.log("Keyin");



        // TUDAT TEST in ELSE
    } else if (text.value) {
        console.log("Bugun");
        today.value = true
        todayList.push(text.value)
        console.log("ishladi Today");
        console.log(todayList);
    }


    // THE INPUT.value CLEANed
    text.value = ""
    check()
}

const check = () => {
    console.log("Check ishladi");
}

onMounted(() => {
    console.log(feature.value);
})
</script>