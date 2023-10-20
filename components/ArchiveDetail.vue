<script setup lang="ts">
import type { Archive } from 'types/Archives'
import { BookmarkDoubleIcon, DownloadIcon, Filter3Icon } from 'tdesign-icons-vue-next'

const props = defineProps<{ item: Archive }>()

const data = computed(() => props.item)

const metadata = computed(() => {
  const metadata = props.item.metadata
  if (metadata.length === 0)
    return []
  return metadata.map((item) => {
    return {
      label: item.label,
      value: item.id,
    }
  })
})
</script>

<template>
  <div class="max-w-[980px] mx-auto sm:px-10 ">
    <div class="b-1   sm:min-w-[600px]  ">
      <div>
        <NuxtLink class="text-gray-600 cursor-pointer hover-underline " @click="$router.back()">
          返回上一级
        </NuxtLink>
      </div>
      <h3 class="text-2xl">
        {{ data.name }}
      </h3>
      <p class="mt--2">
        {{ " >> " }}         {{ data.l1Class }} {{ " / " }} {{ data.l2Class }} {{ " / " }} {{ data.type }}
      </p>
    </div>
    <div class="text-gray-600 flex flex-row gap-3 items-center mt--2 text-[0.9rem]" align="center">
      <p>
        {{ data.downloadCount }} 次下载 <DownloadIcon />
      </p>
      <p>
        {{ data.favCount }} 次收藏 <BookmarkDoubleIcon />
      </p>
      <p>
        {{ data.viewCount }} 次浏览 <Filter3Icon />
      </p>
    </div>
    <div>
      <h3>下载</h3>
      <div v-for="i in metadata" :key="i.value" class=" flex flex-col sm:flex-row bg-white drop-shadow hover:shadow transition-all p-3 px-6 my-3 rounded-xl flex justify-between items-center">
        <p>源：{{ i.label }}</p>
        <TSpace>
          <TButton variant="outline" tag="a" :href="`https://api-nuistshare.dustella.net/api/download?metadata_id=${i.value}`" download rel="noopener noreferrer">
            预览
          </TButton>
          <TButton tag="a" :href="`https://api-nuistshare.dustella.net/api/download?metadata_id=${i.value}`" download rel="noopener noreferrer">
            下载
          </TButton>
        </TSpace>
      </div>
    </div>
  </div>
</template>
