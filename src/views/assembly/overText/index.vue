<template>
  <div class="content-box-mh">
    <h3>CSS 文本超出时显示 tooltips</h3>
    <div class="con">
      <div class="wrap">
        <div class="txt" data-title="这是一段可以自动出现tooltip的文本">较少的文本不会出现tooltip,也不会有省略号</div>
      </div>
      <div class="wrap">
        <div class="txt" data-title="较少的文本不会出现">但是可以滑动下方的滚动条可以看到调整内容的宽度,让较少的文本溢出</div>
      </div>
      <div class="wrap">
        <div class="txt" data-title="只有字数多的时候才出现tooltip，而且还会有省略号">
          只有字数多的时候才出现tooltip，而且还会有省略号,滑动下方的滑块可以看到调整内容的宽度,注意观察文本的变化
        </div>
      </div>
    </div>
    <input
      style="margin-top: 20px"
      type="range"
      min="200"
      value="500"
      max="800"
      oninput="this.previousElementSibling.style.width = this.value + 'px'"
    />

    <h3>CSS 多行文本自动展开收起</h3>
    <div class="con">
      <div class="text-wrap">
        <div class="text-content">
          <label class="expand"><input type="checkbox" hidden /></label>
          对于多行文本,我们可以使用CSS来实现自动展开收起的效果,在这里我们指定当文本的行数为3行且文本溢出时,文本内容显示省略号,并显示展开按钮,点击展开按钮,文本内容会展开,点击收起按钮,文本内容会收起。
        </div>
      </div>
      <div class="text-wrap">
        <div class="text-content">
          <label class="expand"><input type="checkbox" hidden /></label>
          同样的我们也可以滑动下面的滑块来调整内容的宽度,使文本的行数发生变化,文本溢出时使用@container监听css属性变化,使用纯css显示展开和收起按钮,并实现展开收起的效果。
        </div>
      </div>
    </div>
    <input
      style="margin-top: 20px"
      type="range"
      min="200"
      value="500"
      max="800"
      oninput="this.previousElementSibling.style.width = this.value + 'px'"
    />

    <h3>使用封装的组件</h3>
    <div class="con">
      <div class="text-wrap">
        <OverText
          text="实现该效果使用到了两个核心的css新特性:滚动驱动动画和样式查询,我们通过css变量, --trunk 是true或者false来表示文本是否溢出或者未溢出.通过滚动驱动动画我们可以监听到--trunk 是否发生变化."
        />
      </div>
      <div class="text-wrap">
        <OverText
          :line-clamp="2"
          text="对于溢出的文本来说,会执行动画从而使得--trunk 变量从false变为true,然后通过@container查询--trunk 为true的容器，找到指定容器后就可以操作其样式，列如添加伪元素实现tooltip或者展开收起等按钮效果"
        />
      </div>
      <div class="text-wrap">
        <OverText
          :show-tool-tip="false"
          :line-clamp="3"
          :show-expand="true"
          text="这是一个当文本达到3行时，如果文本溢出了，就显示展开或者收起按钮的文本组件，通过滑动下面的滑块，可以控制内容的宽度，从而控制文本的行数。该组件还能控制是否显示tooltip,更多用法，下面的表格中都有描述，可以自行查看。"
        />
      </div>
    </div>
    <input
      style="margin-top: 20px"
      type="range"
      min="200"
      value="500"
      max="800"
      oninput="this.previousElementSibling.style.width = this.value + 'px'"
    />

    <div class="flx-center" style="width: 100%">
      <el-checkbox v-model="showToolTip">显示tooltip</el-checkbox>
      <el-checkbox v-model="showExpand">显示展开按钮</el-checkbox>
      <div class="flx-center">
        <label style="padding: 0 10px; font-size: 14px">行数</label>
        <el-input-number size="small" v-model="lineClamp" :min="1" :max="4" />
      </div>
    </div>
    <div class="con">
      <div class="text-wrap">
        <OverText
          :key="hasChangedTime"
          :show-tool-tip="showToolTip"
          :line-clamp="lineClamp"
          :show-expand="showExpand"
          tooltip-class-name="tooltipClass"
          text="这是一个当文本达到3行时，如果文本溢出了，就显示展开或者收起按钮的文本组件，通过滑动下面的滑块，可以控制内容的宽度，从而控制文本的行数。该组件还能控制是否显示tooltip,更多用法，下面的表格中都有描述，可以自行查看。"
        />
      </div>
    </div>
    <input
      style="margin-top: 20px"
      type="range"
      min="200"
      value="500"
      max="800"
      oninput="this.previousElementSibling.style.width = this.value + 'px'"
    />

    <el-descriptions title="配置项 📚" :column="1" border style="margin: 20px 0">
      <el-descriptions-item label="text"> 组件显示文本</el-descriptions-item>
      <el-descriptions-item label="lineClamp">显示的文本行数 用法同-webkit-line-clamp</el-descriptions-item>
      <el-descriptions-item label="showExpand">
        是否显示展开按钮 默认 false 为true时且lineClamp大于1显示展开按钮
      </el-descriptions-item>
      <el-descriptions-item label="showToolTip"> 是否显示tooltip 默认 true </el-descriptions-item>
      <el-descriptions-item label="tooltipClassName"> tooltip 的样式类名 </el-descriptions-item>
    </el-descriptions>
  </div>
</template>
<script setup lang="ts" name="overText">
import OverText from "@/components/OverText/index.vue";
import { ref, watch } from "vue";
const showToolTip = ref(true);
const showExpand = ref(false);
const lineClamp = ref(2);
const hasChangedTime = ref(new Date().getTime());

watch([showToolTip, showExpand, lineClamp], () => {
  hasChangedTime.value = new Date().getTime();
});
</script>
<style lang="scss" scoped>
@import "./index.scss";
</style>
