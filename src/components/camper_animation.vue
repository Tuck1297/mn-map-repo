<template>
<div id="animation_container">
                                <div class="street"></div>
                                <div class="street-stripe"></div>
                                <div class="street-bottom-curb"></div>
                                <div class="rv"></div>
                                <div class="wheel wheel1"></div>
                                <div class="wheel wheel2"></div>
                                <div class="camper-anim-bg bg-animation"></div>
                            </div>
</template>

<style>

.street {
    background: #7a7a7a url(data:image/png;base64,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);
    height: 80px;
    width: 2000px;
    position: absolute;
    top: 140px;
    left: -100px;
    box-shadow: 0 1px 16px rgba(111, 35, 51, 0.4) inset;
    z-index: 2;
}

.street:after {
    content: '';
    display: block;
    position: absolute;
    width: 2000px;
    height: 10px;
    background: #cdbcb4;
    top: 0px;
    border-bottom: 3px solid #72625a;
    z-index: 1;
}

.street:before {
    content: '';
    display: block;
    position: absolute;
    width: 2000px;
    height: 7px;
    background: #c2a99d;
    top: -7px;
    z-index: 1;
}

.street-bottom-curb {
    content: '';
    display: block;
    position: absolute;
    width: 2000px;
    height: 90px;
    background: #cdbcb4;
    top: 135px;
    left: -100px;
    z-index: 1;
}

.street-stripe {
    background: #d4d4d4;
    height: 8px;
    width: 100px;
    position: absolute;
    top: 184px;
    border-radius: 2px;
    box-shadow: 200px 0 0 #d4d4d4, 400px 0 0 #d4d4d4, 600px 0 0 #d4d4d4, 800px 0 0 #d4d4d4, 1000px 0 0 #d4d4d4, 1200px 0 0 #d4d4d4, 1400px 0 0 #d4d4d4, 1600px 0 0 #d4d4d4, 1800px 0 0 #d4d4d4, 2000px 0 0 #d4d4d4;
    animation: move-stripes linear 1s normal forwards infinite;
    -webkit-animation: move-stripes linear 1s normal forwards infinite;
    -moz-animation: move-stripes linear 1s normal forwards infinite;
    -ms-animation: move-stripes linear 1s normal forwards infinite;
    -o-animation: move-stripes linear 1s normal forwards infinite;
    z-index: 3;
}

@keyframes move-stripes {
    0% {
        transform: translateX(0px);
    }

    100% {
        transform: translateX(-200px);
    }
}

.rv {
    background-image: url('../../images/rv.png');
    background-repeat: no-repeat;
    background-size: cover;
    width: 320px;
    height: 160px;
    position: absolute;
    top: 48px;
    left: 32px;
    z-index: 3;
}

.wheel {
    position: absolute;
    animation: rotate 0.75s infinite forwards linear normal;
    -webkit-animation: rotate 0.75s infinite forwards linear normal;
    -moz-animation: rotate 0.75s infinite forwards linear normal;
    -ms-animation: rotate 0.75s infinite forwards linear normal;
    -o-animation: rotate 0.75s infinite forwards linear normal;

}

@keyframes rotate {
    0% {
        transform: rotate(0deg)
    }

    100% {
        transform: rotate(360deg);
    }
}

.wheel1 {
    background-image: url('../../images/back-wheel.png');
    background-size: contain;
    background-repeat: no-repeat;
    width: 30px;
    height: 30px;
    top: 150px;
    left: 121px;
    z-index: 5;
}

.wheel2 {
    background-image: url('../../images/front-wheel.png');
    background-size: contain;
    background-repeat: no-repeat;
    width: 30px;
    height: 30px;
    top: 150px;
    left: 304px;
    z-index: 5;
}

.camper-anim-bg {
    background-image: url('../../images/layered_peaks.png');
    background-repeat: repeat-x;
    position: absolute;
    top: -200px;
    width: 1600px;
    height: 600px;
    left: -60px;
    z-index: 0;
    display: block;
}

.bg-animation {
    animation: linear 15s rightToLeft normal backwards infinite;
    -webkit-animation: linear 15s rightToLeft normal backwards infinite;
    -moz-animation: linear 15s rightToLeft normal backwards infinite;
    -ms-animation: linear 15s rightToLeft normal backwards infinite;
    -o-animation: linear 15s rightToLeft normal backwards infinite;
}

@keyframes rightToLeft {
    0% {
        opacity: 0;
        transform: translateX(0);
    }

    2% {
        opacity: 1;
    }

    98% {
        opacity: 1;
    }

    100% {
        transform: translateX(-30%);
        opacity: 0;
    }
}
</style>