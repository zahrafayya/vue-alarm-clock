<template>
    <div class="container">
        <div class="item">
            <div class="time">
                08 : 67
            </div>
            <div class="col">
                <div class="alarm-title">
                    <div class="name">
                        my alarm
                    </div>
                    <div class="toggle">

                    </div>
                </div>
                <div class="days">
                    mon tue wed thurs
                </div>
                <div class="ringtone">
                    ringtone
                </div>
                <div class="counter">
                    time left
                </div>
            </div>
        </div>
    </div>
</template>

<script>

</script>

<style>
.item {
    border-radius: 12px;
    border: #BE46DF 2px solid;
    width: 40vw;
    height: fit-content;
    display: flex;
    flex-direction: row;
    align-items: center;

    margin-top: 36px;
    padding: 20px 24px;
}

.time {
    font-size: 64px;
    font-weight: 300;
    font-family: 'Reem Kufi Fun', sans-serif;
}

.col {
    margin: 0px 32px;
}

.alarm-title {
    display: flex;
}

.name {
    font-size: 28px;
    font-family: 'Poppins';
}
    
</style>