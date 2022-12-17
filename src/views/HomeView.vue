<template>
  <div class="main">
    <div>
      <a-button type="primary" @click="showCatalog"><van-button type="primary" />show catalog</a-button>
      <a-modal v-model:visible="visible" width="100%" wrap-class-name="full-modal">
        <template #footer>
          <a-button key="submit" type="primary" @click="handleOk">返回</a-button>
        </template>
        <Catalog :catalogData="catalogData" @catalogClick="handleCatalogClick" />
      </a-modal>
    </div>
    <div id="bookContent"></div>
  </div>
</template>

<script setup lang="ts">
import ePub, { type NavItem } from 'epubjs'
import Catalog from '@/components/Catalog.vue'
import { ref } from 'vue'
import type Navigation from 'epubjs/types/navigation';

const book = ePub("/埃勒里·奎因 30本合集.epub");
const visible = ref<boolean>(false);
let catalogData = ref<NavItem[]>([])

const rendition = book.renderTo("bookContent", {
  width: '100vw',
  height: '100vh',
  manager: "continuous",
  flow: "scrolled"
});
let rUrl: string = ""

rendition.display();
let bookRenderedEvent = function (section: any) {
  console.log('rurl', section.href);

  rUrl = section.href
}
rendition.on("rendered", bookRenderedEvent)
// 这个事件在开始的时候就会暴露一个路径

function showCatalog() {
  visible.value = true;
  book.loaded.navigation.then(function (nav) {
    console.log("catalog array:", nav);
    catalogData.value = nav?.toc;

  })

}
const handleCatalogClick = (href: string) => {

  const reg = /^(([a-z]+\/)+)?(.+\.x?html)$/ig
  let prevHref = rUrl.replace(reg, '$1')
  console.log('jump', prevHref, "hey", href);
  rendition.display(prevHref + href);
}
const handleOk = () => {
  visible.value = false;
}
</script>
<style lang="scss">
.main {
  display: flex;
  flex-direction: column;

}

.full-modal {
  .ant-modal {
    max-width: 100%;
    top: 0;
    padding-bottom: 0;
    margin: 0;
  }

  .ant-modal-content {
    display: flex;
    flex-direction: column;
    height: calc(100vh);
  }

  .ant-modal-body {
    flex: 1;
  }
}
</style>


