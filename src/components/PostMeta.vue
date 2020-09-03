<template>
  <div class="post-meta">
    Posted {{ post.date }}.
    <template v-if="post.cjkWordCount">
      {{ post.cjkWordCount }} words.
      <strong>
        {{ post.cjkReadTime }} min read.
        <span :id="post.path" class="leancloud_visitors" :data-flag-title="post.title">
          <i class="leancloud-visitors-count">0</i>
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
  mounted() {
    if (process.env.NODE_ENV === 'production') {
      window.Valine = require('valine');
      const valine = new Valine({
        el: '#vcomments',
        appId: 'ValcujOd8RqQw9PnuSaVkWey-gzGzoHsz',
        appKey: 'xHr7ovH5p80YCEyIi5QMAB9F',
        recordIP: true,
        visitor: true,
        requiredFields: ['nick', 'mail'],
        placeholder: '欢迎留言',
      })
    }
  },
}
</script>

<style lang="scss">
.post-meta {
  font-size: 0.8em;
  opacity: 0.8;
}
</style>
