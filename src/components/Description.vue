<template>
  <div class='hanoi-description'>
    <p>
      The <b>Tower of Hanoi</b> (also called the Tower of Brahma or Lucas' Tower and sometimes pluralized as Towers) is a mathematical game or puzzle. It consists of three rods and a number of disks of different sizes, which can be placed onto any rod. The puzzle starts with the disks in a neat stack in ascending order of size on one rod, the smallest at the top, thus making a conical shape.
    </p>
    <slot />
    <ul class="hanoi-description__rules">
      <li>Only one disk can be moved at a time.</li>
      <li>Each move consists of taking the upper disk from one of the stacks and placing it on top of another stack or on an empty rod.</li>
      <li>No larger disk may be placed on top of a smaller disk.</li>
    </ul>
    <br>
    <hr>
    <br>
    Just drag item onto new position or click (tap) item and then where you want it to go

  </div>
</template>

<script>
  export default {
    
  }
</script>

<style lang='scss'>
  .hanoi-description {
    text-align: justify;
    padding: 0 10px;

    &__rules {
      margin-top: 15px;
      padding-left: 15px;
    }
  }
</style>