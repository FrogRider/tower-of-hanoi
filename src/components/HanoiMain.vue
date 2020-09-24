<template>
  <div class="">
    <Description>
    <div class="hanoi-field">
      <div 
        class="hanoi-field__tower"
        v-for="tower in towers"
        :key="tower"
        @dragover="dragOver(tower)"
        @click.prevent="towerClicked(tower)"
      >
        <div 
          class="hanoi-field__element"
          :class="{'selected': isSelected(el.id)}"
          v-for="el in elements[tower]"
          :key="el.id"
          :style="el.styles"
          :draggable="el.dragable"
          @dragstart="dragStart(el)"
          @dragend="dragEnd()"
          @click="elementClicked(el)"
        >

        </div>
      </div>
    </div>
    </Description>
  </div>
</template>

<script>
import Description from '@/components/Description.vue'

export default {
  components: {Description},
  data: function () {
    return {
      blocksMaxAmount: 8,
      blocksAmount: 8,
      colors: ['#f3722c', '#f8961e', '#f9844a', '#f9c74f', '#90be6d', '#43aa8b', '#4d908e', '#577590'],
      elements: [[],[],[]],
      draggedElement: null,
      towers: [0,1,2],
      newPosition: null
    }
  },
  methods: {
    isSelected (id) {
      if(this.draggedElement && this.draggedElement.id === id) return true
    },

    elementClicked (el) {
      if(this.draggedElement && el.dragable) this.draggedElement = null
      else if(el.dragable) this.draggedElement = el
    },

    towerClicked (towerId) {
      if(this.getTopElemId(towerId) > this.draggedElement.id) {
        const oldTower = this.draggedElement.towerId
        this.elements[oldTower].splice(0,1)
        this.elements[towerId].unshift(this.draggedElement)
        this.draggedElement.towerId = towerId
        this.newPosition = this.draggedElement = null
      } 
    },

    fillBlocks () {
      [...Array(this.blocksAmount).keys()].forEach(i => {
        this.elements[0].push(
          {
            id: i, 
            towerId: 0, 
            styles: `--width: ${20 + 10 * i}%; --bgc: ${this.colors[i]}`,
            dragable: false,
            isDraging: false
          }
        )
      })
    },

    getTopElemId (tower) {
      const selectedTower = this.elements[tower]
      if (selectedTower.length > 0)
        return selectedTower[0].id
      else return 7
    },

    dragStart (el) {
      if(el.dragable) this.draggedElement = el
      //add block to curent draging boofer and remove from tower
    },

    dragEnd () {
      //append element to the towerId it have
      if (this.newPosition !== null) {
        const oldTower = this.draggedElement.towerId
        this.elements[oldTower].splice(0,1)
        this.elements[this.newPosition].unshift(this.draggedElement)
        this.draggedElement.towerId = this.newPosition
        this.newPosition = this.draggedElement = null
      }
    },

    dragOver (towerId) {
      if(this.getTopElemId(towerId) >= this.draggedElement.id) {
        this.newPosition = towerId
      }
    }
  },
  watch: {
    blocksAmount: function () {
      this.elements = []
      this.fillBlocks()
    },
    elements: function () {
      this.elements.forEach(tower => {
        tower.forEach(block => {
          if (tower.indexOf(block) === 0) block.dragable = true
          else block.dragable = false
        })
      })
    }
  },
  mounted () {
    this.fillBlocks()
  }
};
</script>

<style lang="scss">
  .hanoi-field {
    max-width: 600px;
    width: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: space-evenly;
    height: 200px;
    position: relative;
    background-color: rgba(231, 231, 231, .5);

    &::after {
      content: '';
      position: absolute;
      width: 100%;
      height: 5px;
      background-color: #000;
      bottom: -5px;
      border-radius: 2px;
    }

    &__tower {
      width: 30%;
      height: 100%;
      // background-color:red;
      position: relative;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-end;
      z-index: 2;

      &::after {
        content: '';
        width: 3px;
        height: 100%;
        position: absolute;
        top: 0;
        left: 50%;
        background-color: #000;
        transform: translateX(-50%);
        z-index: 1;
      }
    }

    &__element {
      width: var(--width);
      height: 20px;
      border-radius: 3px;
      position: relative;
      overflow: hidden;

      &.selected::after {
        opacity: .7;
      }

      &::after {
        content: '';
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: var(--bgc);
        z-index: 2;
      }
    }
  }
</style>