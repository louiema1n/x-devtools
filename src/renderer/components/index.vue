<template>
    <div class="layout">
        <Layout>
            <Header>
                <Menu mode="horizontal" theme="light" active-name="1">
                    <div class="layout-logo">
                        <Icon type="md-construct" size="28"/>
                    </div>
                    <div class="layout-nav">
                        <MenuItem name="1">
                            <Icon type="ios-navigate"></Icon>
                            Item 1
                        </MenuItem>
                        <MenuItem name="2">
                            <Icon type="ios-keypad"></Icon>
                            Item 2
                        </MenuItem>
                    </div>
                </Menu>
            </Header>
            <Layout>
                <Sider hide-trigger :style="{background: '#fff'}">
                    <Menu active-name="1-2" theme="light" width="auto" :open-names="['1']">
                        <Submenu name="1">
                            <template slot="title">
                                <Icon type="ios-navigate"></Icon>
                                Item 1
                            </template>
                            <MenuItem name="1-1">Option 1</MenuItem>
                            <MenuItem name="1-2">Option 2</MenuItem>
                            <MenuItem name="1-3">Option 3</MenuItem>
                        </Submenu>
                        <Submenu name="2">
                            <template slot="title">
                                <Icon type="ios-keypad"></Icon>
                                Item 2
                            </template>
                            <MenuItem name="2-1">Option 1</MenuItem>
                            <MenuItem name="2-2">Option 2</MenuItem>
                        </Submenu>
                        <Submenu name="3">
                            <template slot="title">
                                <Icon type="ios-analytics"></Icon>
                                Item 3
                            </template>
                            <MenuItem name="3-1">Option 1</MenuItem>
                            <MenuItem name="3-2">Option 2</MenuItem>
                        </Submenu>
                    </Menu>
                </Sider>
                <Layout :style="{padding: '5px'}">
                    <Content :style=contentStyle>
                        Content
                    </Content>
                </Layout>
            </Layout>
        </Layout>
    </div>
</template>

<script>
  import {Layout, Header, Menu, MenuItem, Submenu, Sider, Icon} from 'iview'

  export default {
    name: 'landing-page',
    components: {Layout, Header, Menu, MenuItem, Submenu, Sider, Icon},
    data () {
      return {
        contentStyle: {padding: '24px', minHeight: document.documentElement.clientHeight - 84 + 'px', background: '#fff'}
      }
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      resizeHandle () {
        this.contentStyle.minHeight = document.documentElement.clientHeight - 84 + 'px'
      }
    },
    mounted: function () {
      window.addEventListener('resize', this.resizeHandle)
    },
    beforeDestroy: function () {
      window.removeEventListener('resize', this.resizeHandle)
    }
  }
</script>

<style scoped>
    .ivu-layout-header {
        width: 100%;
        background: #fff;
        padding: 0px;
    }

    .layout {
        background: #f5f7f9;
        position: relative;
        overflow: hidden;
    }

    .layout-logo {
        width: 100px;
        border-radius: 3px;
        float: left;
        position: relative;
        left: 3%;
    }

    .layout-nav {
        background: #fff;
        float: right;
        margin-right: 3%;
        position: relative;
    }
</style>
