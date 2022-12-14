<template>
    <div class="aaa">
        <ul>
            <li v-for="item in catalogData" :key="item.id" @click.stop="catalogClick(item.href)">
                {{ item.label }}
                <Catalog v-if="item.subitems.length > 0" :catalogData="item.subitems" @catalogClick="catalogClick" />
            </li>
        </ul>
    </div>
</template>

<script lang="ts" setup>
import { onMounted, reactive, ref } from 'vue';
const emit = defineEmits(["catalogClick"]);

const { catalogData } = defineProps(["catalogData"])
const catalogClick = (href: string) => {
    emit("catalogClick", href);
}



</script>

<style lang="scss" scoped>
.aaa {
    border: 1px red solid;
    height: 200px;
    overflow: auto;
}
</style>