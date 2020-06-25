<template>
    <div>
        <Menu v-if="menus&&menus.length>0"
              :active-name="activeName"
              :theme="theme"
              width="auto"
              :open-names="openNames"
              accordion
              @on-select="clickMenuItem">
            <Submenu v-for="(item,key) in menus.filter(item2=>item2['parentId']==='0')" :name="item['id']" :key="key">
                <template slot="title">
                    <Icon :type="item['icon']"></Icon>
                    {{item['title']}}
                </template>
                <MenuItem v-for="(item1,key1) in menus.filter(item2=>item2['parentId']===item['id'])"
                          :name="item1['id']" :key="key1"
                          @on-select="clickMenuItem(item1)">{{item1['title']}}
                </MenuItem>
            </Submenu>
        </Menu>
    </div>
</template>

<script>
    export default {
        name: "index",
        props: {
            menus: Array,
            activeName: String,
            openNames: Array,
        },
        data(){
          return {
              theme:'light'
          }
        },
        methods: {
            clickMenuItem(id) {
                this.$emit('clickOpenTabPane', id)
            }
        }
    }
</script>

<style scoped>

</style>