<template>
  <div class="hero">

    <div
      class="hero__left"
      ref="left"
    >
      <img
        src="../assets/images/baptiste.svg"
        alt="Baptiste"
        ref="leftImage"
      >
    </div>

    <joystick ref="joystick" />

    <div
      class="hero__right"
      ref="right"
    >
      <img
        src="../assets/images/dumas.svg"
        alt="Dumas"
        ref="rightImage"
      >
    </div>

  </div>
</template>

<script>
import { TimelineLite, Power4 } from "gsap/all";
import Joystick from "./Joystick";

export default {
  name: "Hero",
  components: {
    Joystick
  },
  mounted() {
    const left = this.$refs.left;
    // const leftImage = this.$refs.leftImage;
    const right = this.$refs.right;
    const joystick = this.$refs.joystick.$el;

    let timeline = new TimelineLite({ paused: true });

    let yDuration = "1";

    const viewPortSize = {
      height: document.documentElement.clientHeight,
      width: document.documentElement.clientWidth
    };

    let leftBoundingClientRect = left.getBoundingClientRect();

    timeline
      .to(
        left,
        {
          top: viewPortSize.height - 200 + "px",
          duration: yDuration,
          ease: Power4.easeInOut
        },
        "animateY"
      )
      .to(
        right,
        {
          bottom: "200px",
          duration: yDuration,
          ease: Power4.easeInOut,
          onComplete: function() {
            leftBoundingClientRect = left.getBoundingClientRect();

            console.log(leftBoundingClientRect);

            console.log(joystick.style);

            joystick.style.left = leftBoundingClientRect.right - 58 + "px";
            joystick.style.top = leftBoundingClientRect.top - 58 + "px";
          }
        },
        "animateY"
      )
      .to(joystick, {
        opacity: 1,
        duration: 0.4,
        ease: "linear",
        delay: -0.5
      });

    setTimeout(() => {
      timeline.resume();
    }, 1000);
  }
};
</script>
