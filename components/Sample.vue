<template lang="pug">
  .container
    .moon
      .carter.first
      .carter.second
      .carter.third
      .moon-halo

    .meteors
      - var n = 0;
      while (n++ < 4)
        .meteor

    .stars
      - var n = 0;
      while (n++ < 25)
        .star

    .planet

    .mountains


</template>

<style lang="scss">

  // Mixins
  @mixin translate_center {
    transform: translate(-50%, -50%);
  }


  @mixin iterate_stars {
    @for $i from 1 through $count_star {
      &:nth-child(#{$i}) {
        opacity: random() + 0.1;
        transform: scale(random() + 0.1);
        &:after {
          transform: translate(random() * -100 + px, random() * -120 + px) scale(random() * 1.2);
        }

        &:before {
          transform: translate(random() * 80 + px, random() * 60 + px) scale(random() * 1.2);
        }
      }
    }
  }


  /* Style */

  body {
    padding: 0;
    margin: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: Roboto, sans-serif;
  }


  .container {
    width: 500px;
    height: 500px;
    position: relative;
    border-radius: 50%;
    background-color: $color_sky;
    overflow: hidden;
    z-index: -100;
    border: 20px solid white;
    box-shadow: 0 8px 20px rgba(darken($purple, 20%), 0.2);


    *,
    *:before,
    *:after{
      position: absolute;
      box-sizing: border-box;
    }
  }

  .moon {
    width: $size_moon;
    height: $size_moon;
    z-index: 4;
    box-shadow: 0 0 10px rgba($color_moon, 0.5);
    background-color: $color_moon;
    border-radius: 50%;
    top: 50%;
    left: 50%;
    @include translate_center;

    .carter {
      width: 70px;
      height: 70px;
      background-color: $color_carter;
      border-radius: 50%;
      left: 60%;
      top: 50%;
      @include translate_center;
      z-index: 3;

      &.second {
        width: 27px;
        height: 27px;
        left: 84%;
        top: 55%;
        z-index:2;
      }

      &.third {
        width: 40px;
        height: 40px;
        left: 65%;
        top: 75%;
        z-index: 1;
      }
    }

    .moon-halo {
      &:after,
      &:before {
        content: '';
        display: inline-block;
      }

      &,
      &:after,
      &:before {
        top: 50%;
        left: 50%;
        @include translate_center;
        background-color: #fff;
        opacity: 0.2;
        width: $size_moon + 50;
        height: $size_moon + 50;
        border-radius: 50%;
        transform-origin: 0 0;
        animation: shine 1s ease-in-out infinite alternate;
      }

      &:before {
        width: $size_moon + 100;
        height: $size_moon + 100;
        animation-delay: 200ms;

      }

      &:after {
        width: $size_moon + 150;
        height: $size_moon + 150;
        animation-delay: 400ms;
      }
    }
  }

  .meteors {
    z-index: -10;
    width: 100%;
    height: 100%;

    .meteor {
      width: 0;
      height: 0;
      border-top: 1px solid transparent;
      border-bottom: 1px solid transparent;
      border-right: 90px solid $color_meteor;
      border-image: linear-gradient(
          to left,
          $color_meteor,
          transparent
      ) 1 100%;
      border-radius: 0 2px 2px 0;
      transform: rotate(30deg);
      position: absolute;
      top: 23%;
      left: 70%;
      opacity: 0;
      animation: shooting 4s linear infinite;
      animation-delay: 1s;

      &:nth-child(2) {
        top: 13%;
        left: 64%;
        border-right-width: 80px;
        animation-delay: 2s;
      }

      &:nth-child(3) {
        top: 23%;
        left: 62%;
        border-right-width: 50px;
        animation-delay: 2.5s;
      }

      &:nth-child(4) {
        top: 64%;
        left: 72%;
        border-right-width: 50px;
        animation-delay: 1.3s;
      }
    }
  }

  .stars {
    z-index: -11;
    width: 100%;
    height: 100%;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-gap: 1em;

    .star {
      position: relative;

      &:after,
      &:before {
        content: '';
        display: inline-block;
      }

      &,
      &:after,
      &:before {
        width: 4px;
        height: 4px;
        background-color: #fff;
        border-radius: 50%;
      }

      @include iterate_stars;
    }
  }


  .planet {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background: linear-gradient($color_planet_up, $color_planet_middle, $color_planet_bottom);
    top: 14%;
    left: 6%;
    animation: breathe 1.5s ease-in-out infinite alternate;
  }

  .mountains {
    width: 100%;
    bottom: 0;
    left: 0;
    right: 0;
    height: 40%;
    z-index: 5;

    &:before,
    &:after {
      content: "";
      background: linear-gradient(to bottom, $color_mountain_dark 5%, $color_mountain_light);
      display: inline-block;
      width: 400px;
      height: 400px;
      border-radius: 40px;
      transform: rotate(45deg);
      box-shadow: 2px -2px 0 $color_mountain_silhouette;
    }

    &:before {
      left: -15%;
      top: 40%;
    }

    &:after {
      left: 33%;
      top: 55%;
    }


  }

  .info{
    position: absolute;
    bottom: 0;
    width: 100%;
    margin-top: 10px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px 0;

    .info_col{
      position: relative;
      padding: 0 20px;
      a, b{
        position: relative;
      }
      a{
        color: $purple;
        text-decoration: none;
      }

      b{
        color: #a5a5a5;
      }
    }
  }

  /* Animations */

  @keyframes breathe {
    from {
      transform: translate(0, -2%);
    }

    to {
      transform: translate(0, 2%);
    }
  }

  @keyframes shine {
    from {
      transform: scale(1) translate(-50%, -50%);
    }

    to {
      transform: scale(1.02) translate(-50%, -50%);
    }
  }

  @keyframes shooting {
    0% {
      transform: rotate(30deg) translate(-100%, -100%);
      opacity: 1;
    }

    20% {
      transform: rotate(30deg) translate(100%, 100%);
      opacity: 0;
    }
    100% {
      transform: rotate(30deg) translate(100%, 100%);
      opacity: 0;
    }
  }
</style>
