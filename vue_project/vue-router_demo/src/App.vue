<template>
  <div>
    <ul>
      <li v-for="item in links"><a @click='$goRoute(item.route)'>{{item.text}}</a></li>
    </ul>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      links: [
        {
          text: '胡萝卜',
          route: '/home'
        },
        {
          text: '大白菜',
          route: '/page01'
        },
        {
          text: '水蜜桃',
          route: '/page02'
        }
      ]
    }
  }
}
</script>

<style>
  .text-center {
    text-align: center;
  }
  ul li{
    list-style: none;
    display: inline-block;
    margin-right: 10px;
  }
  ul li a {
    display: inherit;
    padding: 5px 10px;
    border:1px solid #ccc;
  }
   ul li a:hover {
     cursor: pointer;
     color: #fff;
     background: #138bec;
     border:1px solid #138bec;
   }
</style>
