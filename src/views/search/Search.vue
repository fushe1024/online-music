<script setup name="Search">
import SearchSongList from '@/components/song-ls/SearchSongList.vue'
import { smoothScrollToTop } from '@/utils/usertools'
import { ref, onMounted } from 'vue'
import { reqSearch } from '@/api/search'
import { useRoute } from 'vue-router'
const route = useRoute()

// 搜索参数对象
const searchObj = ref({
  keywords: route.query.keywords, // 搜索关键字
  limit: 30, // 每页条数
  offset: 1, // 当前页
  type: 1 // 类型
})

// 歌曲列表
const songlist = ref([]) // 数据列表
const total = ref(0) // 总数

// 获取数据
const getSongList = async () => {
  try {
    const { data } = await reqSearch(searchObj.value)
    songlist.value = data.result.songs
    total.value = data.result.songCount
  } catch (error) {
    console.error('Error fetching search songlist data:', error.message)
  }
}

// 切换分页
const changeCurrent = () => {
  getSongList()
  smoothScrollToTop()
}

onMounted(() => {
  getSongList()
})
</script>

<template>
  <div class="search-pages">
    <div class="search-head">
      <div class="search-input">
        <a-input-search
          @press-enter="getSongList()"
          @search="getSongList()"
          :allow-clear="true"
          v-model="searchObj.keywords"
          placeholder="请输入歌名、歌词、歌手或专辑"
        />
      </div>
    </div>
    <div class="search-content">
      <div class="inside-container">
        <a-tabs default-active-key="1">
          <a-tab-pane key="1" title="单曲">
            <!-- 歌曲列表 -->
            <SearchSongList :songlist="songlist" />
            <!-- 分页器 -->
            <div class="pagination">
              <a-pagination
                @change="changeCurrent"
                v-model:current="searchObj.offset"
                show-total
                :show-jumper="false"
                :page-size="searchObj.limit"
                :total="total"
              />
            </div>
          </a-tab-pane>
          <a-tab-pane key="2" title="歌手"> 待完善 </a-tab-pane>
          <a-tab-pane key="3" title="专辑"> 待完善 </a-tab-pane>
          <a-tab-pane key="4" title="视频"> 待完善 </a-tab-pane>
          <a-tab-pane key="5" title="歌词"> 待完善 </a-tab-pane>
          <a-tab-pane key="6" title="歌单"> 待完善 </a-tab-pane>
          <a-tab-pane key="7" title="声音主播"> 待完善 </a-tab-pane>
          <a-tab-pane key="8" title="用户"> 待完善 </a-tab-pane>
        </a-tabs>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.search-pages {
  // 搜索头部
  .search-head {
    height: 260px;
    background: url('@/assets/images/search_banner.png') no-repeat;
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;

    .search-input {
      width: 600px;
      height: 50px;

      // 搜索框
      &:deep(.arco-input-search) {
        height: inherit;
        background: #000;
        border-radius: 4px;
        font-size: 14px;
        border: none;
        outline: none;

        // 输入框文本颜色
        .arco-input {
          color: $primary-color;
        }
        // 输入框提示文本颜色
        .arco-input::placeholder {
          color: $secondary-color;
        }
        // 搜索图标
        .arco-input-suffix {
          font-size: 20px;
          color: $secondary-color;

          // 隐藏搜索图标背景
          .arco-icon-hover::before {
            display: none;
          }

          .arco-icon-hover:hover {
            color: $primary-color;
          }
        }
      }
    }
  }

  // 搜索内容
  .search-content {
    // 内部容器
    .inside-container {
      margin: 50px auto 30px;
      width: 1200px;

      // tab样式
      &:deep(.arco-tabs) {
        color: $secondary-color;

        // 关闭nav 白色下线条
        .arco-tabs-nav::before {
          display: none;
        }

        // tab-nav
        .arco-tabs-nav-tab {
          // 基本样式
          .arco-tabs-tab {
            font-size: 16px;
            font-weight: 400;
            color: $secondary-color;
          }

          // 激活tab下划线样式
          .arco-tabs-nav-ink {
            height: 1px;
            background-color: $primary-color;
          }

          // 激活tab文字样式
          .arco-tabs-tab.arco-tabs-tab-active {
            color: $primary-color;
          }

          // 关闭悬停伪类
          .arco-tabs-tab .arco-tabs-tab-title::before {
            display: none;
          }
        }

        // content样式
        .arco-tabs-content {
          padding-top: 25px;
        }
      }

      // 分页器
      .pagination {
        margin-top: 30px;
        display: flex;
        justify-content: center;

        &:deep(.arco-pagination) {
          .arco-pagination-list {
            .arco-pagination-item {
              color: $secondary-color;
              font-size: 16px;
              border-radius: 0;

              &:hover {
                color: $hover-color;
                background-color: transparent;
              }

              &.arco-pagination-item-active {
                color: $primary-color;
                background-color: $hover-color;
              }
            }
          }
        }
      }
    }
  }
}
</style>
