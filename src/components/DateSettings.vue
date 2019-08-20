<template>
    <div class="prompt-date-block">
        <div class="date-block-inner">
            <h3>Deadline settings</h3>
            <div>If you have deadline for this task, you can put it here, and we will remember you it at all time!</div>
            <div class="date-inputs-block">
                <input type="text" placeholder="Month" v-model="currentMonth">
                <input type="text" placeholder="Date" v-model="currentDate">
                <input type="text" placeholder="Hour" v-model="currentHour">
                <input type="text" placeholder="Minute" v-model="currentMinute">
            </div>
            <div class="date-button-block">
                <div @click="cancelDeadline" class="cancel-date-button date-button">No, thanks</div>
                <div @click="checkDateData" class="commit-date-button date-button">Commit</div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: "DateSettings",
        props: {

        },
        data: function () {
            return {
                //date: new Date(),
                //currentTime: ,
                currentMonth: new Date().getMonth() + 1,
                currentDate: new Date().getDate(),
                currentHour: '',
                currentMinute: '',
            }
        },
        methods: {
            cancelDeadline: function () {
                this.$emit('cancelDeadline',{
                    timer: {
                        days: '',
                        hours: '',
                        minutes: '',
                        license: false
                    }
                })
            },
            checkDateData: function () {
                let time = new Date();
                const monthDurations = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
                this.currentMinute = parseInt(this.currentMinute);
                this.currentHour = parseInt(this.currentHour);
                this.currentDate = parseInt(this.currentDate);
                this.currentMonth = parseInt(this.currentMonth) - 1;
                console.log(this.currentDate);
                if(12 > this.currentMonth > 0 && 0 < this.currentDate < 32 && 0 <= this.currentHour < 24 && 0 <= this.currentMinute < 60){
                    let timerForDeadline = this.timerGoing(monthDurations, time, {month: this.currentMonth, day: this.currentDate, hours: this.currentHour, minutes: this.currentMinute})
                    this.$emit('deadlineInstruction',{
                        timer: timerForDeadline
                    })
                }
                else{
                    alert('Invalid input')
                }
            },
            timerGoing: function (array, nowTime, deadline) {
                let timeForDeadline = {
                    days: 0,
                    hours: 0,
                    minutes: 0
                }
                timeForDeadline.minutes = timeForDeadline.minutes + (deadline.minutes + deadline.hours * 60) - (nowTime.getMinutes() + ( nowTime.getHours() * 60 ))
                timeForDeadline.minutes = timeForDeadline.minutes + (deadline.day * 24 * 60) - (nowTime.getDate() * 24 * 60)
                for (let i = nowTime.getMonth(); i < deadline.month; i++){
                    timeForDeadline.minutes = timeForDeadline.minutes + (array[i] * 24 * 60)
                }
                if(timeForDeadline.minutes >= 60){
                    timeForDeadline.hours = timeForDeadline.hours + Math.floor(timeForDeadline.minutes / 60)
                    timeForDeadline.minutes = timeForDeadline.minutes % 60
                }
                if(timeForDeadline.hours >= 24){
                    timeForDeadline.days = timeForDeadline.days + Math.floor(timeForDeadline.hours / 24)
                    timeForDeadline.hours = timeForDeadline.hours % 24
                }
                return timeForDeadline
            }
        }
    }
</script>

<style scoped>
    .prompt-date-block {
        width: 80vw;
        height: 40vh;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background: white;
        border: 8px solid black;
        border-radius: 3vw;
    }
    .date-block-inner {
        width: 90%;
        margin: 0 auto;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: flex-start;
    }
    .date-block-inner h3{
        margin: 0;
    }
    .date-inputs-block {
        width: 100%;
        display: flex;
        justify-content: space-between;
    }
    .date-inputs-block input{
        width: 22%;
        border: none;
        border-bottom: 2px solid black;
        transition: border .5s ease;
    }
    .date-inputs-block input:focus{
        border-bottom: 2px solid #00b600;
    }
    .date-button-block {
        width: 100%;
        display: flex;
        justify-content: space-between;
    }
    .date-button {
        height: 6vh;
        color: white;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 2vh;
        font-size: 1.2rem;
    }
    .date-button:hover{
        cursor: pointer;
    }
    .cancel-date-button {
        width: 36%;
        background: #e90000;
    }
    .commit-date-button {
        width: 30%;
        background: #23dd07;
    }

    @media screen and (max-width: 500px){
        .cancel-date-button {
            font-size: 1rem;
        }
        .commit-date-button {
            font-size: 1rem;
        }
    }
</style>