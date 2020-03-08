<template>
  <div class="hero">

    <div
      class="hero__loading-screen"
      ref="loadingScreen"
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

  </div>
</template>

<script>
import { gsap, Power3, RoughEase } from "gsap/all";
import Joystick from "./Joystick";

export default {
  name: "Hero",
  data() {
    return {
      verticalSeperationPoint: 180
    };
  },
  components: {
    Joystick
  },
  methods: {
    moveJoystick(x, y) {
      const joystick = this.$refs.joystick.$el;

      //Move joystick to correct position
      joystick.style.left = x + "px";
      joystick.style.top = y + "px";
    },
    initAnimation() {
      const {
        darkTitle,
        lightTitle,
        titleContainer,
        content,
        loadingScreen,
        animatedTextRightSvg,
        animatedTextLeftSvg,
        animatedTextLeft
      } = this.$refs;

      const joystick = this.$refs.joystick.$el;

      //Scroll to top of page on load and prevent scroll
      const body = document.getElementsByTagName("body")[0];
      body.classList.add("noscroll");
      window.scrollTo(0, 0);

      let timeline = gsap.timeline({
        paused: true,
        onComplete: () => {
          //enable scroll after animation finishes
          body.classList.remove("noscroll");
        }
      });

      let yDuration = "1";

      const viewPortSizeHeight = document.documentElement.clientHeight;

      //Where the left section will animate down to
      let leftTopFinishPosition =
        viewPortSizeHeight - this.verticalSeperationPoint;

      const flickeringTextPoints = 50;
      const flickeringTextDuration = 0.5;

      timeline
        .to(loadingScreen, {
          opacity: 0,
          duration: 0,
          onComplete: () => {
            //Get postion of left section - calculating after animation has started so element is fully rendered. Wasn't working properly above
            let leftBoundingClientRect = animatedTextLeft.getBoundingClientRect();

            //Move joystick to initial position
            this.moveJoystick(
              leftBoundingClientRect.right - 58,
              leftTopFinishPosition - 58
            );

            //Have to vertically center the images with javascript to make them fully responsive
            //Get the size of the image inside it's container
            let leftSvgBoundingClientRect = animatedTextLeftSvg.getBoundingClientRect();

            //Find the difference between that and the viewport and half it
            const imageMargin =
              (viewPortSizeHeight - leftSvgBoundingClientRect.height) / 2 +
              "px";

            //Vertically center images in container
            animatedTextLeftSvg.style.top = imageMargin;
            animatedTextRightSvg.style.bottom = imageMargin;

            //Hide loading screen
            loadingScreen.style.display = "none";
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
            bottom: this.verticalSeperationPoint + "px",
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
    },
    initJoystickResize() {
      const { animatedTextLeft, animatedTextLeftSvg } = this.$refs;

      window.addEventListener("resize", () => {
        const viewPortSizeHeight = document.documentElement.clientHeight;

        let leftTopFinishPosition =
          viewPortSizeHeight - this.verticalSeperationPoint;
        let leftBoundingClientRect = animatedTextLeft.getBoundingClientRect();

        requestAnimationFrame(() => {
          this.moveJoystick(
            leftBoundingClientRect.right - 58,
            leftTopFinishPosition - 58
          );

          animatedTextLeftSvg.style.top = leftTopFinishPosition + "px";
        });
      });
    }
  },
  mounted() {
    this.initAnimation();
    this.initJoystickResize();
  }
};
</script>
