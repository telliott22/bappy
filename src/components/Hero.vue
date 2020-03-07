<template>
  <div class="hero">

    <div
      class="hero__signature"
      ref="signature"
    >

    </div>

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
import { TimelineMax, Power4, RoughEase } from "gsap/all";
import Joystick from "./Joystick";

export default {
  name: "Hero",
  components: {
    Joystick
  },
  mounted() {
    const left = this.$refs.left;
    const leftImage = this.$refs.leftImage;
    const right = this.$refs.right;
    const rightImage = this.$refs.rightImage;
    const joystick = this.$refs.joystick.$el;
    const signature = this.$refs.signature;

    let timeline = new TimelineMax({ paused: true });

    let yDuration = "1";

    const viewPortSizeHeight = document.documentElement.clientHeight;

    //Used to calcuate where on the screen the sections will meet vertically
    let verticalSeperationPoint = 180;

    //Where the left section will animate down to
    let leftTopFinishPosition = viewPortSizeHeight - verticalSeperationPoint;

    //Get postion of left section
    let leftBoundingClientRect = left.getBoundingClientRect();

    //Move joystick to correct position
    joystick.style.left = leftBoundingClientRect.right - 58 + "px";
    joystick.style.top = leftTopFinishPosition - 58 + "px";

    const flickeringTextPoints = 50;
    const flickeringTextDuration = 0.5;

    timeline
      .to(signature, {
        opacity: 0,
        duration: 0,
        onComplete: () => {
          signature.style.display = "none";
        }
      })
      .fromTo(
        leftImage,
        {
          opacity: 0
        },
        {
          opacity: 1,
          duration: flickeringTextDuration,
          ease: RoughEase.ease.config({
            template: Power4.easeInOut,
            strength: 2,
            points: flickeringTextPoints,
            taper: "out",
            randomize: false,
            clamp: true
          }),
          delay: "0.1"
        },
        "flickeringText"
      )
      .fromTo(
        rightImage,
        {
          opacity: 0
        },
        {
          opacity: 1,
          duration: flickeringTextDuration,
          ease: RoughEase.ease.config({
            template: Power4.easeInOut,
            strength: 2,
            points: flickeringTextPoints,
            taper: "out",
            randomize: false,
            clamp: true
          })
        },
        "flickeringText"
      )
      .to(
        left,
        {
          top: leftTopFinishPosition + "px",
          duration: yDuration,
          ease: Power4.easeInOut
        },
        "animateY"
      )
      .to(
        right,
        {
          bottom: verticalSeperationPoint + "px",
          duration: yDuration,
          ease: Power4.easeInOut
        },
        "animateY"
      )
      .to(joystick, {
        opacity: 1,
        duration: 0.4,
        ease: Power4.easeOut,
        delay: -0.4
      });

    setTimeout(() => {
      timeline.resume();
    }, 2000);
  }
};
</script>
