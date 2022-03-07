<template>
  <div class="post-meta">
    Posted {{ post.date }}.
    <template v-if="post.cjkWordCount">
      {{ post.cjkWordCount }} words.
      <strong>
        {{ post.cjkReadTime }} min read.
        <span :id="post.path" class="leancloud_visitors" :data-flag-title="post.title">
          <i class="leancloud-visitors-count"><font-awesome :icon="['fa', 'spinner']" pulse /></i>
        </span> views.
        <!-- <font-awesome :icon="['fa', 'spinner']" pulse v-if="loading" /> -->
        <!-- <span v-else>{{ hitCount }}</span> views. -->
      </strong>
    </template>
  </div>
</template>

<script>
export default {
  props: ['post'],
  data(){
    return {
      waline:null
    }
  },
  mounted() {
    window.Waline = require('@waline/client');
    let vm = this
    vm.$nextTick(()=>{
      vm.waline = Waline({
        el: '#vcomments',
        serverURL: 'waline.zhengzexin.com',
        visitor: true,
        requiredMeta: ['nick', 'mail'],
        path: this.$route.path,
      })
    })
  },
  watch: {
    $route (to, from) {
      if (from.path != to.path) {
        this.waline && this.waline.setPath(to.path)
      }
    }
  }
}
</script>

<style lang="scss">
.post-meta {
  font-size: 0.8em;
  opacity: 0.8;
}
</style>
