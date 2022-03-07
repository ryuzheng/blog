<template>
  <div>
    <h1 class="friends-title text-center space-bottom">Friends & Guestbook</h1>

    <div class="friends-container text-center space-bottom">
      <a
        class="btn"
        v-for="friend in friends"
        :key="friend.id"
        :href="friend.link"
        target="_blank"
        :style="friend.style"
        ><g-image class="avatar" alt="avatar" :src="friend.avatar" blur="5" />
        <span>{{ friend.id }}</span></a
      >
      <p class="note">
        * 欢迎各位来留言
      </p>
    </div>

    <div class="post-comments">
      <!-- <div id="disqus_thread" /> -->
      <div id="vcomments"></div>
    </div>
  </div>
</template>

<script>

export default {
  metaInfo: {
    title: 'Friends',
  },
  data() {
    return {
      friends: [
        {
          id: '走遍世界的随想',
          link: 'http://synyan.net/',
          avatar: 'https://gravatar.loli.net/avatar/bae7710d5271fe63d4dfcf86965afbda?s=160',
          style: 'color: #fff; background-color: #473922',
        },
        {
          id: '极客导航',
          link: 'http://blog.it2048.cn/',
          avatar: 'https://gravatar.loli.net/avatar/d41d8cd98f00b204e9800998ecf8427e?s=160',
          style: 'color: #fff; background-color: #b59672',
        },
        {
          id: '刘兰涛的技术博客',
          link: 'http://blog.liulantao.com/',
          avatar: 'https://avatars3.githubusercontent.com/u/153937?s=160',
          style: 'color: #fff; background-color: #595058',
        },
        {
          id: '简约设计の艺术',
          link: 'http://88250.b3log.org/',
          avatar: 'https://img.hacpai.com/avatar/1353745196354_1556262761146.png?imageView2/1/w/256/h/256/interlace/0/q/100',
          style: 'color: #fff; background-color: #9f8cd1',
        },
        {
          id: '小彭博客',
          link: 'http://www.houfukude.tk/',
          avatar: 'http://www.gravatar.com/avatar/bd1eb4f5f7616591fb72751f1307841a?s=160&d=monsterid',
          style: 'color: #fff; background-color: #005240',
        },
        {
          id: '码农明明桑',
          link: 'https://isming.info',
          avatar: 'https://secure.gravatar.com/avatar/a9508e0578974654d1348e9b6bfbc8ed?s=160&d=mm&r=g',
          style: 'color: #fff; background-color: #487747',
        },
        {
          id: '沉舟侧畔',
          link: 'https://springwood.me/',
          avatar: 'https://gravatar.loli.net/avatar/e7073f269e803311f4be6994f1086ed2?s=160&d=mm&r=g',
          style: 'color: #fff; background-color: #5e2c38',
        },
        {
          id: 'YuZhangWang的领域',
          link: 'https://www.yuzhang.wang/',
          avatar: 'https://cdn.jsdelivr.net/gh/YuZhangWang/Creative_pictures01@main/img/20210907012640.png',
          style: 'color: #fff; background-color: #577C8A',
        },
      ],
    }
  },
  mounted() {
    if (process.env.NODE_ENV === 'production') {
      window.Waline = require('@waline/client');
      const waline = Waline({
        el: '#vcomments',
        serverURL: 'waline.zhengzexin.com',
        visitor: true,
        requiredMeta: ['nick', 'mail'],
      })
    }
  },
}
</script>

<style lang="scss">
.friends-container {
  max-width: var(--content-width);
  margin-left: auto;
  margin-right: auto;

  .btn {
    margin: 0.3em 0.6em 0.3em 0;
    text-decoration: none;
    padding: 0.5em;
    border-radius: var(--radius);
    display: inline-block;
    overflow: hidden;
    position: relative;

    .avatar {
      position: absolute;
      top: 0;
      left: 0;
      bottom: 0;
      width: 48px;
      height: 48px;
      border-radius: var(--radius);
      background-color: #fff;
      transform: rotate(20deg) translate(-15%, 0%);
    }

    span {
      margin-left: 2.6em;
    }
  }

  .note {
    margin: calc(var(--space) / 2) 0;
    font-size: 0.9em;
    opacity: 0.8;
  }
}

.post-comments {
  padding: calc(var(--space) / 2);
  max-width: var(--content-width);
  margin: 0 auto;
  font-family: var(--base-font-family);
}
</style>
