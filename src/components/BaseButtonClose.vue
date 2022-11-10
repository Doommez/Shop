<template>
  <div class="cl-btn-3">
    <span class="top" />
    <span class="bot" />
  </div>
</template>

<script setup>

</script>

<style lang="scss" scoped>
  .cl-btn-3 {
    width: 16px;
    height: 16px;
    position: relative;
    transform: rotate(45deg) scale(2);
    .top,.bot{
      display: block;
      background: #337AB7;
      position: absolute;
      top: 50%;
      left: 50%;
      animation-timing-function: cubic-bezier(1, 0, 0, 1);
      animation-iteration-count: 1;
    }
    .top {
      height: 100%;
      width: 2px;
      margin-left: -1px;
      transform: skewY(45deg);
      top: 0;
    }
    .bot {
      width: 100%;
      height: 2px;
      margin-top: -1px;
      transform: skewX(-45deg);
      left: 0;
    }
    &:hover {
      cursor: pointer;
      .top {
        animation: top-out 0.25s forwards, top-in 0.25s forwards 0.25s;
      }
      .bot {
        animation: bot-out 0.25s forwards 0.1s, bot-in 0.25s forwards 0.35s;
      }
    }
  }

  @-webkit-keyframes top-out {
    from {
      top: 0%;
      height: 100%;
    }
    to {
      top: 200%;
      height: 0%;
    }
  }

  @keyframes top-out {
    from {
      top: 0%;
      height: 100%;
    }
    to {
      top: 200%;
      height: 0%;
    }
  }
  @-webkit-keyframes top-in {
    from {
      top: -100%;
      height: 0%;
    }
    to {
      top: 0%;
      height: 100%;
    }
  }
  @keyframes top-in {
    from {
      top: -100%;
      height: 0%;
    }
    to {
      top: 0%;
      height: 100%;
    }
  }
  @-webkit-keyframes bot-out {
    from {
      left: 0%;
      width: 100%;
    }
    to {
      left: 200%;
      width: 0%;
    }
  }
  @keyframes bot-out {
    from {
      left: 0%;
      width: 100%;
    }
    to {
      left: 200%;
      width: 0%;
    }
  }
  @-webkit-keyframes bot-in {
    from {
      left: -100%;
      width: 0%;
    }
    to {
      left: 0%;
      width: 100%;
    }
  }
  @keyframes bot-in {
    from {
      left: -100%;
      width: 0%;
    }
    to {
      left: 0%;
      width: 100%;
    }
  }
</style>
