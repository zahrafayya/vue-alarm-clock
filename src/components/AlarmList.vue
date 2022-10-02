<template>
    <div class="container">
        <div class="set">
            <div class="hour">
                <button class="button up" @click="upHour">
                    ^
                </button>
                <div class="time-display">
                    <div class="time-digit">
                    {{ hour_1 }}
                </div>
                    <div class="time-digit">
                        {{ hour_2 }}
                    </div>
                </div>
                <button class="button down" @click="downHour">
                    v
                </button>
            </div>
            <div class="titikdua">
                :
            </div>
            <div class="minute">
                <button class="button up" @click="upMinute">
                    ^
                </button>
                <div class="time-display">
                    <div class="time-digit">
                    {{ minute_1 }}
                </div>
                    <div class="time-digit">
                        {{ minute_2 }}
                    </div>
                </div>
                <button class="button down" @click="downMinute">
                    v
                </button>
            </div>
        </div>
        <button class="add" @click="addAlarm">
            Add
        </button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            hour_1: 0,
            hour_2: 0,
            minute_1: 0,
            minute_2: 0,
            ring: false,

            alarms: [ { id: 0, hour: 0, minute: 0 }]
        }
    },  
    methods: {
        upHour() {
            if(this.hour_2 == 9) this.hour_1++;
            this.hour_2 = (this.hour_2 == 9) ? 0 : this.hour_2 + 1;
            if(this.hour_1 == 3) this.hour_1 = 0;

            if(this.hour_1 == 2 && this.hour_2 == 4)
            {
                this.hour_1 = 0;
                this.hour_2 = 0;
            }
        },
        downHour() {
            if(this.hour_1 == 0 && this.hour_2 == 0)
            {
                this.hour_1 = 2;
                this.hour_2 = 3;
            }
            else 
            {
                if(this.hour_2 == 0)
                {
                    this.hour_2 = 9;
                    this.hour_1--;
                }
                else this.hour_2--;
            }
        },
        upMinute() {
            if(this.minute_2 == 9)
            {
                this.minute_2 = 0;
                this.minute_1++;
            }
            else this.minute_2++;
            
            if(this.minute_1 == 6) {
                this.minute_1 = 0;
                this.upHour();
            }
        },
        downMinute() {
            if(this.minute_1 == 0 && this.minute_2 == 0)
            {
                this.downHour();
                this.minute_1 = 5;
                this.minute_2 = 9;
            }
            else 
            {
                if(this.minute_2 == 0)
                {
                    this.minute_2 = 9;
                    this.minute_1--;
                }
                else this.minute_2--;
            }
        },
        checkTime()
        {
            const today = new Date();
            let _hour = today.getHours();
            let _minute = today.getMinutes();
            
            if(
                _hour == (this.hour_1 * 10) + this.hour_2 && 
                _minute == (this.minute_1 * 10) + this.minute_2
            )
            {
                if(!this.ring)
                {
                    alert("Alarm is ringing!");
                    this.ring = true;
                }
            }
            else this.ring = false;
        },
        addAlarm() {
            let _hour = this.hour_1 * 10 + this.hour_2;
            let _minute = this.minute_1 * 10 + this.minute_2;

            this.alarms.push({ id: 1, hour: _hour, minute: _minute});

            console.log(this.alarms);
        } 
    },
    mounted() {
        setInterval(() => this.checkTime(), 1000);
    }                                
}
</script>
    
<style>
.container
{
    display: flex;
    flex-direction: column;
    align-items: center;
}
.set
{
    background: linear-gradient(135deg, #f403d1, #64b5f6);
    border-radius: 12px;
    padding: 18px;
    display: flex;
    align-items: center;
}
.titikdua
{
    font-size: 64px;
    font-weight: 800;
    font-family: 'Reem Kufi Fun', sans-serif;
    padding-bottom: 24px;
    margin: 0 8px
}
.time-display
{
    display: flex;
}
.time-digit
{
    font-size: 64px;
    background: #1A1A1A;
    color: #FFF;
    display: flex;
    padding: 0 16px;
    height: 100px;
    justify-content: center;
    align-items: center;
    margin: 12px 4px;
    border-radius: 4px;

}
.add
{
    margin-top: 34px;
    display: flex;
    font-weight: 600;
    font-size: 24px;
    justify-content: center;
    align-items: center;
    width: 132px;
    height: 42px;
    border-radius: 18px;
    border: #A663E5 1px solid;
}

</style>