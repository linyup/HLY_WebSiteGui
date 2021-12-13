<template>
  <Sider :style="{position: 'fixed', height: '100vh', left: 0, overflow: 'auto'}">
    <div class="logo-con">
        <span style="line-height: 60px; font-weight: 700; font-size: 20px; color: white;">
          HELIOS WebGuide
        </span>
    </div>
    <Menu theme="light" width="auto" @on-select="anchor" @on-open-change="handleopen" :accordion="true"
          :open-names="['submenu-1']">
      <Submenu name="submenu-1" v-show="isShowWebsite">
        <template slot="title">
          <Icon style="color: #fff" type="md-planet" />
          <span>速用网址导航</span>
        </template>
        <MenuItem v-for="item in $store.state.websiteList" :name="item.id" :key="item.id">
          <span>{{item.name}}</span>
        </MenuItem>
      </Submenu>
      <Submenu name="submenu-2" v-show="!isShowWebsite">
        <template slot="title">
          <Icon style="color: #fff" type="md-options" />
          <span>网址管理</span>
        </template>
        <MenuItem name="group-manage">
          <Icon style="color: #fff" type="ios-pricetags" />
          <span>网址标签</span>
        </MenuItem>
        <MenuItem name="website-manage">
          <Icon style="color: #fff" type="md-desktop" />
          <span>网址详情</span>
        </MenuItem>
      </Submenu>
      <Submenu name="submenu-3" v-show="!isShowWebsite">
        <template slot="title">
          <Icon style="color: #fff" type="ios-people" />
          <span>用户管理</span>
        </template>
        <MenuItem name="user-manage">
          <Icon style="color: #fff" type="md-walk" />
          <span>用户详情</span>
        </MenuItem>
      </Submenu>

    </Menu>
  </Sider>
</template>

<script>
    export default {
        name: 'SiderMenu',
        computed:{
            isShowWebsite(){
              if (this.$route.meta.title === "websiteguide"){
                return true
              }
            },
        },
        methods: {
            anchorId(id) {
                return 'anchor' + id // querySelector锚点跳转方法id不能为纯数字
            },
            anchor(name) {
                // menuitem是网址分组数据的才执行滚动方法
                if (typeof name === 'number') {
                    const anchorId = this.anchorId(name)
                    document.querySelector(`#${anchorId}`).scrollIntoView({behavior: 'smooth'})
                } else {
                    this.$router.push({name: name})
                }
            },
            handleopen(active) {
                if (active[0] === 'submenu-1') {
                    this.$Spin.show()
                    this.$router.push({name: 'websites'})
                    this.$Spin.hide()
                }
            }
        }
    }
</script>

<style scoped>
  span {
    color: #fff;
  }

  .logo-con {
    padding: 0 8px;
    text-align: center;
    align-items: center;
    height: 60px;
    width: auto;
  }

  .ivu-layout-sider {
    transition: all .2s ease-in-out;
    position: relative;
    background: #0a246a;
    min-width: 0;
  }

  .ivu-menu-light {
    background: #0a246a;
  }

  .ivu-menu-light.ivu-menu-vertical .ivu-menu-item-active:not(.ivu-menu-submenu) {
    color: #2d8cf0;
    background: #2d8cf0;
    z-index: 2;
  }

</style>
