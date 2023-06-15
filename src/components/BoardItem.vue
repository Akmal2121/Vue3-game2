<template>
  <span @click="select(field.id)" :class="getClasses"></span>
</template>

<script>
import { computed } from "vue";
export default {
  name: "BoardItem",
  props: {
    field: {
      type: Object,
      required: true,
    },
    gameStatus: {
      type: Number,
      default: 0,
      required: false,
    },
  },
  setup(props) {
    const getClasses = computed(() => {
      let classes = "item ";
      if (
        (props.field.value === 1 && props.gameStatus === 1) ||
        (props.field.clicked && props.field.value === 1)
      ) {
        classes += "item-active";
      }

      if (props.field.clicked && props.field.value === 0) {
        classes += "item-wrong";
      }
      return classes;
    });

    return {
      getClasses,
    };
  },
  methods: {
    select(id) {
      if (this.gameStatus === 2) {
        this.$emit("selectField", id);
      }
    },
  },
};
</script>

<style scoped>
.item {
  position: relative;
  cursor: pointer;
  width: 50px;
  height: 50px;
  background: #ccc;
  display: inline-block;
  margin: 5px;
  transition: 0.4s;
  transform-style: preserve-3d;
}

.item-active {
  background-color: #42b983cc;
  transform: rotateX(180deg);
}

.item-wrong {
  background-color: #bf5836cc;
  transform: rotateX(180deg);
}
</style>
