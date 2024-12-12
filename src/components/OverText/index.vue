<template>
  <div
    class="over-line-text"
    :class="[lineClamp && lineClamp > 1 ? 'over-text-clamp' : 'over-text', tooltipClassName]"
    :data-title="text"
  >
    <label v-show="showExpand && lineClamp && lineClamp > 1" class="expand"><input type="checkbox" hidden /></label>
    {{ text }}
  </div>
</template>

<script setup lang="ts">
import { toRefs } from "vue";
const props = defineProps({
  text: {
    type: String,
    default: ""
  },
  showExpand: {
    type: Boolean,
    default: false
  },
  showToolTip: {
    type: Boolean,
    default: true
  },
  lineClamp: {
    type: Number
  },
  tooltipClassName: {
    type: String
  }
});

const { lineClamp, showToolTip, showExpand } = toRefs(props);
</script>

<style lang="scss" scoped>
.over-text {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  --show-tooltip: false;
  --show-expand: false;
  animation: check 1s;
  animation-timeline: scroll(x self); // 滚动驱动动画 self表示监听自身滚动，默认是最近的祖先滚动容器
}

.over-text-clamp {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: v-bind(lineClamp);
  overflow: hidden;
  --show-tooltip: false;
  --show-expand: false;
  animation: check 1s;
  animation-timeline: scroll(self); // 滚动驱动动画 self表示监听自身滚动，默认是最近的祖先滚动容器
}

@keyframes check {
  from,
  to {
    --show-tooltip: v-bind(showToolTip);
    --show-expand: v-bind(showExpand);
  }
}

@container style(--show-tooltip: true) {
  //样式查询 查询--trunk 为true的容器
  .over-line-text::after {
    content: attr(data-title);
    position: absolute;
    top: 0;
    width: fit-content;
    left: 50%;
    margin: auto;
    transform: translate(-50%, -100%);
    background-color: rgba(0, 0, 0, 0.6);
    padding: 0.3em 1em;
    border-radius: 4px;
    color: #fff;
    opacity: 0;
    visibility: hidden;
    transition: 0.2s 0.1s;
    width: 100%;
    white-space: wrap;
  }
  .over-line-text:hover::after {
    cursor: default;
    opacity: 1;
    visibility: visible;
  }
}

.expand {
  font-size: 80%;
  line-height: 20px;
  padding: 0 0.5em;
  background-color: #9747ff;
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
  float: right;
  clear: both;
  display: none;
}

@container style(--show-expand: true) {
  .expand {
    display: initial;
  }
}

.expand::after {
  content: "展开";
}

.over-line-text::before {
  content: "";
  float: right;
  height: calc(100% - 22px); /**依赖于 父容器 或者 祖先容器的高度 如果为0 设置祖先flex */
}
.over-line-text:has(:checked) {
  -webkit-line-clamp: 999;
}
.over-line-text:has(:checked) .expand::after {
  content: "收起";
}
.over-line-text:has(:checked) .expand {
  display: initial;
}
</style>
