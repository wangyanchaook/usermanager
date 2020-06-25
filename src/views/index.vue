<style scoped>
    .layout {
        border: 1px solid #d7dde4;
        background: #f5f7f9;
        position: relative;
        border-radius: 4px;
        overflow: hidden;
    }

    .layout-logo {
        width: 100px;
        height: 30px;
        background: #5b6270;
        border-radius: 3px;
        float: left;
        position: relative;
        top: 15px;
        left: 20px;
    }

    .layout-nav {
        width: 420px;
        margin: 0 auto;
        margin-right: 20px;
    }
</style>
<template>
    <div class="layout">
        <Layout>
            <Header>
                <t-header></t-header>
            </Header>
            <Layout>
                <Sider hide-trigger :style="{background: '#fff',height:screenHeight+'px'}">
                    <t-sider
                            :menus="menus"
                            :activeName="activeName"
                            :openNames="openNames"
                            @clickOpenTabPane="clickOpenTabPane"
                    ></t-sider>
                </Sider>
                <Layout :style="{padding: '0 24px 24px'}">
                    <div :style="{margin: '5px 0'}">
                        <ButtonGroup style="margin-right:5px " v-for="(item,key) in openTabPane">
                            <Button :icon="item['icon']" :type="item['type']" @click="setOpenTabPane(item['id'])"
                                    :key="key">{{item['title']}}
                            </Button>
                            <Button v-if="key!==0" :type="item['type']" icon="md-close"
                                    @click="closeTag(item['id'])"></Button>
                        </ButtonGroup>
                    </div>
                    <Content :style="{padding: '10px', minHeight: '280px', background: '#fff'}">
                        <Tabs :animated="false" v-model="tabId" style="margin-top:-15px; ">
                            <div v-for="(item,key) in openTabPane" :key="key">
                                <TabPane v-if="key===0" label="" name="0">这是首页</TabPane>
                                <TabPane v-else :label="''" :name="item['id']">{{item['title']}}</TabPane>
                            </div>
                        </Tabs>
                    </Content>
                </Layout>
            </Layout>
        </Layout>
    </div>
</template>
<script>
    import tHeader from './layouts/header/index'
    import tSider from './layouts/sider/index'

    export default {
        components: {tHeader, tSider},
        data() {
            return {
                screenHeight: document.documentElement.clientHeight - 66,
                timer: false,
                tabId: '0',
                activeName: '',
                openNames: [],
                tabPaneLabel: (h, params) => {
                    return h('Button',
                        {},
                        '11')
                },
                openTabPane: [{title: '首页', id: '0', icon: 'ios-home-outline', parentId: '0'}],
                tagColumns: [
                    {title: '首页', id: '0'},
                    {title: '菜单1', id: '1'}
                ],
                menus: [
                    {title: 'item 1', id: '1', icon: 'ios-navigate', parentId: '0'},
                    {title: 'options 1-1', id: '1-1', icon: 'ios-navigate', parentId: '1'},
                    {title: 'options 1-2', id: '1-2', icon: 'ios-navigate', parentId: '1'},
                    {title: 'options 1-3', id: '1-3', icon: 'ios-navigate', parentId: '1'},
                    {title: 'item 2', id: '2', icon: 'ios-keypad', parentId: '0'},
                    {title: 'options 2-1', id: '2-1', icon: 'ios-navigate', parentId: '2'},
                    {title: 'options 2-2', id: '2-2', icon: 'ios-navigate', parentId: '2'},
                    {title: 'options 2-3', id: '2-3', icon: 'ios-navigate', parentId: '2'},
                    {title: 'item 3', id: '3', icon: 'ios-analytics', parentId: '0'},
                    {title: 'options 3-1', id: '3-1', icon: 'ios-navigate', parentId: '3'},
                    {title: 'options 3-2', id: '3-2', icon: 'ios-navigate', parentId: '3'},
                    {title: 'options 3-3', id: '3-3', icon: 'ios-navigate', parentId: '3'}
                ]
            }
        },
        methods: {
            clickOpenTabPane(id) {
                if (this.openTabPane.length > 0) {
                    let data = this.openTabPane.filter(item => item['id'] === id)
                    if (data.length > 0) {
                    } else {
                        let data = this.menus.filter(item => item['id'] === id)
                        if (data.length > 0) {
                            this.openTabPane.push(data[0])
                        }
                    }
                } else {
                    let data = this.menus.filter(item => item['id'] === id)
                    if (data.length > 0) {
                        this.openTabPane.push(data[0])
                    }
                }
                this.tabId = id
                this.removeButtonType(id)
            },
            removeButtonType(id) {
                this.openTabPane.forEach(item => {
                    if (item['id'] === id) {
                        item['type'] = 'primary'
                    } else {
                        item['type'] = 'default'
                    }
                })
            },
            setOpenTabPane(id) {
                this.tabId = id
                this.removeButtonType(this.tabId)
            },
            closeTag(id) {
                this.openTabPane = this.openTabPane.filter(item => item['id'] !== id)
                if (this.openTabPane.length > 0) {
                    this.tabId = this.openTabPane[this.openTabPane.length - 1]['id']
                    this.removeButtonType(this.tabId)
                } else {
                    this.tabId = '0'
                }
            },
        },
        mounted() {
            this.removeButtonType(this.tabId)
            // const that = this
            // 监听窗口大小
            window.onresize = () => {
                return (() => {
                    this.screenHeight = document.documentElement.clientHeight - 66
                })()
            }
        }
    }
</script>
