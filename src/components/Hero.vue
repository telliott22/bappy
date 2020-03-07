<template>
  <div class="hero">

    <div
      class="hero__loading-screen"
      ref="signature"
    >

      <div class="hero__signature">

      </div>

      <small>Loading</small>

    </div>

    <div
      class="hero__animated-text hero__animated-text--left"
      ref="animatedTextLeft"
    >

      <div
        class="hero__title"
        ref="titleContainer"
      >
        <h1 ref="lightTitle">Baptiste Dumas</h1>
        <h1 ref="darkTitle">Baptiste Dumas</h1>
      </div>

      <div
        class="hero__content"
        ref="content"
      >

        <p> I’m a digital designer who like analysing problems and solving them. I have always seen design as an exciting challenge to solve the web’s usability, where beautiful aesthetics and functionality exist in harmony. I design interfaces to be used with high attention to detail. I love geometry, architecture and generative design. I'm intrigued by design process and how to continually enhance it. I have also been a proud member of the Awwwards Jury panel since January 2016 lorem situm estate posum.</p>

      </div>

      <img
        src="../assets/images/baptiste.svg"
        alt="Baptiste"
        ref="animatedTextLeftSvg"
      >
    </div>

    <joystick ref="joystick" />

    <div
      class="hero__animated-text hero__animated-text--right"
      ref="animatedTextRight"
    >
      <img
        src="../assets/images/dumas.svg"
        alt="Dumas"
        ref="animatedTextRightSvg"
      >
    </div>

    <!-- <div class="hero__content hero__content--left">

      <div class="hero__title">
        <h1>Baptiste Dumas</h1>
      </div>

    </div> -->

  </div>
</template>

<script>
import { gsap, Power3, RoughEase } from "gsap/all";
import Joystick from "./Joystick";

export default {
  name: "Hero",
  components: {
    Joystick
  },
  mounted() {
    const animatedTextLeft = this.$refs.animatedTextLeft;
    const animatedTextLeftSvg = this.$refs.animatedTextLeftSvg;
    // const animatedTextRight = this.$refs.animatedTextRight;
    const animatedTextRightSvg = this.$refs.animatedTextRightSvg;
    const joystick = this.$refs.joystick.$el;
    const signature = this.$refs.signature;
    const content = this.$refs.content;

    const { darkTitle, lightTitle, titleContainer } = this.$refs;

    let timeline = gsap.timeline({ paused: true });

    let yDuration = "1";

    const viewPortSizeHeight = document.documentElement.clientHeight;

    //Used to calcuate where on the screen the sections will meet vertically
    let verticalSeperationPoint = 180;

    //Where the left section will animate down to
    let leftTopFinishPosition = viewPortSizeHeight - verticalSeperationPoint;

    //Get postion of left section
    let leftBoundingClientRect = animatedTextLeft.getBoundingClientRect();

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
        animatedTextLeftSvg,
        {
          opacity: 0
        },
        {
          opacity: 1,
          duration: flickeringTextDuration,
          ease: RoughEase.ease.config({
            template: Power3.easeInOut,
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
        animatedTextRightSvg,
        {
          opacity: 0
        },
        {
          opacity: 1,
          duration: flickeringTextDuration,
          ease: RoughEase.ease.config({
            template: Power3.easeInOut,
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
        animatedTextLeftSvg,
        {
          top: leftTopFinishPosition + "px",
          duration: yDuration,
          ease: Power3.easeInOut
        },
        "animateY"
      )
      .to(
        animatedTextRightSvg,
        {
          bottom: verticalSeperationPoint + "px",
          duration: yDuration,
          ease: Power3.easeInOut
        },
        "animateY"
      )
      .to(
        titleContainer,
        {
          opacity: 1,
          duration: 0.1,
          delay: "0.2"
        },
        "animateY"
      )
      .to(joystick, {
        opacity: 1,
        duration: 0.4,
        ease: Power3.easeOut,
        delay: -0.4
      })
      .to(
        content,
        {
          opacity: 1,
          y: 0,
          duration: 0.4,
          ease: Power3.easeOut,
          delay: -0.5
        },
        "animateContent"
      )
      .to(
        lightTitle,
        {
          rotationX: "90deg",
          duration: 0.4,
          delay: -0.7
        },
        "animateContent"
      )
      .to(
        darkTitle,
        {
          rotationX: "0deg",
          y: "6px",
          duration: 0.4,
          delay: -0.5
        },
        "animateContent"
      );

    setTimeout(() => {
      timeline.resume();
    }, 2000);
  }
};
</script>
