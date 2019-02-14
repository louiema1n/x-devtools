<template>
    <div class="layout">
        <Layout>
            <Header >
                <Menu mode="horizontal" theme="light" active-name="1">
                    <div class="layout-logo">
                        <Icon type="md-construct" size="28"/>
                    </div>
                    <div class="layout-nav">
                        <MenuItem name="1">
                            <Icon type="ios-navigate"></Icon>
                            需求管理
                        </MenuItem>
                        <MenuItem name="2">
                            <Icon type="ios-keypad"></Icon>
                            待开发
                        </MenuItem>
                    </div>
                </Menu>
            </Header>
            <Layout>
                <Sider hide-trigger :style="{background: '#fff'}">
                    <Menu active-name="1" theme="light" width="auto" :open-names="['1']">
                        <MenuItem v-for="(name, index) in requireNames" name="1"><Icon type="ios-navigate"></Icon>
                                {{name}}</MenuItem>
                    </Menu>
                </Sider>
                <Layout :style="{padding: '5px'}">
                    <Content :style=contentStyle>
                        <div style="background:#f5f7f9;padding: 1vh">
                            <Card :bordered="false">
                                <p slot="title">需求文档
                                    <Icon type="md-checkmark-circle" size="20"/>
                                </p>
                                <p>
                                    <Icon type="ios-document" />
                                    2019年双节报表固化需求.docx</p>
                            </Card>
                            <Card :bordered="false">
                                <p slot="title">测试文档
                                    <Icon type="md-close-circle" size="20"/>
                                </p>
                            </Card>
                            <Card :bordered="false">
                                <p slot="title">其他文档
                                </p>
                            </Card>
                            <Card :bordered="false">
                                <p slot="title">所有文档
                                </p>
                                <p v-for="file in totalFiles"><Icon type="ios-document" />{{file}}</p>
                            </Card>
                        </div>
                    </Content>
                    <Affix :offset-bottom="5" style="text-align: right">
                        <Button type="primary" shape="circle" icon="md-add" @click="addRequirement"></Button>
                    </Affix>
                </Layout>
            </Layout>
        </Layout>
    </div>
</template>

<script>
  import {Layout, Header, Menu, MenuItem, Submenu, Sider, Icon, Card, Affix, Button} from 'iview'
  import {ipcRenderer} from 'electron'

  export default {
    name: 'landing-page',
    components: {Layout, Header, Menu, MenuItem, Submenu, Sider, Icon, Card, Affix, Button},
    data () {
      return {
        contentStyle: {minHeight: document.documentElement.clientHeight - 84 + 'px', background: '#f5f7f9'},
        totalFiles: [],
        requireNames: []
      }
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      resizeHandle () {
        this.contentStyle.minHeight = document.documentElement.clientHeight - 84 + 'px'
      },
      addRequirement () {
        ipcRenderer.send('open-directory-dialog', 'openDirectory')
      },
      getPath (e, path) {
        if (path == null) {
          alert('请选择一个文件夹')
        } else {
          let fs = require('fs')
          let all = this.totalFiles = []
          let reName = this.requireNames = []
          fs.readdir(path, function (err, files) {
            if (err) {
              return console.error(err)
            }
            // 处理需求名称
            reName.push(path.substr(path.lastIndexOf('\\') + 1))
            // 处理全部文件
            files.forEach(function (file) {
              all.push(file)
            })
          })
        }
      }
    },
    mounted: function () {
      window.addEventListener('resize', this.resizeHandle)
      let fs = require('fs')
      fs.access(global.userDataPath + '/test.json', function (exists) {
        console.log(exists)
      })
      // 绑定回调方法 - 处理选择文件夹
      ipcRenderer.on('selectedItem', this.getPath)
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
    .ivu-icon.ivu-icon-md-checkmark-circle {
        float: right;
        color: #19be6b;
    }
    .ivu-icon.ivu-icon-md-close-circle {
        float: right;
        color: #ed4014;
    }
    .ivu-card {
        margin-bottom: 1vh;
    }
</style>
