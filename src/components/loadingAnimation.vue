<template>
    <div class="loading_slate">
      <div class="animation_wrap">
        <div class="hypnotic"></div>
      </div>
  </div>
</template>
<style>

.loading_slate {
  position: absolute;
  background-color: rgba(255, 255, 255, 0.5);
  z-index: 1;
  width: 100vw;
  height: 100vh;
}

.animation_wrap {
  display: flex; 
  justify-content: center;
  margin-top: 300px;
}

.hypnotic {
   width: 105.6px;
   height: 44px;
   border: 3.5px solid #4352a5;
   border-radius: 50%;
   display: grid;
   animation: hypnotic-ds34hk 2.8s infinite linear;
}

.hypnotic:before,
.hypnotic:after {
   content: "";
   grid-area: 1/1;
   border: inherit;
   border-radius: 50%;
   animation: inherit;
   animation-duration: 4.199999999999999s;
}

.hypnotic:after {
   --s: -1;
}

@keyframes hypnotic-ds34hk {
   100% {
      transform: rotate(calc(var(--s,1)*1turn));
   }
}
</style>