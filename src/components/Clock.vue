<template>
    <div class="banner">
        <div class="date">
            {{ day }}, {{ date }} {{ month }} {{ year }}
        </div>
        <div class="clock">
            {{ hour }} : {{ minute }} : {{ second }}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            hour: 0,
            minute: 0,
            second: 0,
            day: null,
            date: null,
            month: null,
            year: null,
        }
    },  
    methods: {
        updateTime() {
            const dayNames = [null, "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];
            const monthNames = ["January", "February", "March", "April", "May", "June",
                                "July", "August", "September", "October", "November", "December"];
            const today = new Date();
            let _hour = today.getHours();
            let _minute = today.getMinutes();
            let _second = today.getSeconds();

            this.day = dayNames[today.getDay()];
            this.date = today.getDate();
            this.month = monthNames[today.getMonth()];
            this.year = today.getFullYear();

            this.hour = (_hour < 10) ? '0' + _hour : _hour;
            this.minute = (_minute < 10) ? '0' + _minute : _minute;
            this.second = (_second < 10) ? '0' + _second : _second;
        }
    },                                              
    mounted() {
        setInterval(() => this.updateTime(), 1000);
    },
    beforeMount()
    {
        this.updateTime();
    }
}
</script>

<style>
.banner {
    width: 100%;
    height: 32vh;
    position: relative;
    padding-bottom: 18px;

    background: linear-gradient(135deg, #f403d1, #64b5f6);
    
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.date {
    font-size: x-large;
    font-family: 'Poppins';
    margin-bottom: 78px;
    justify-content: center;
    align-items: center;
}

.banner::before {
    content: '';
    position: absolute;
    background: linear-gradient(135deg, #f403d1, #64b5f6);
    top: -5px;
    left: -5px;
    bottom: -30px;
    right: -5px;
    z-index: -1;
    filter: blur(30px);
    border-radius: 40px;
    max-width: 100vw;
}

.clock {
    font-size: 160px;
    font-weight: 800;
    font-family: 'Reem Kufi Fun', sans-serif;
    height: auto;
    display:table;
}
</style>