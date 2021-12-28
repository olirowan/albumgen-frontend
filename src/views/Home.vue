<template>
  <div class="home">
    <div class="">
      <div class="project-pallete-red is-size-1 is-capitalized">
        Get Random Selections
      </div>
      <div class="project-pallete-red is-size-2 is-capitalized">
        <span class="is-lowercase is-size-4">From</span> The 500 Greatest Albums
        of All Time
      </div>
    </div>
  </div>
</template>

<style>
html {
  background: url("~@/assets/landing.jpg") no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

#first {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;

  justify-items: center;
  text-align: center;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
  width: 100% !important;
}
</style>
