<template>
<ul id="meTree">
    <li v-for='(item,index) in treeData' :treeData="item.children" :key='index'>
        <div class="tree-item" :style='treeStyle'>
           <span class="tree-icon" v-if="item.children" @click.stop.self='openTree(item)'>+</span><span class="tree-item" @click='treeItemCallBack(item)'>{{item.title}}</span>
        </div>
    <me-tree :tree-data='item.children' :add-count='count' v-if='item.children' v-show='item.childrenShow' @toParent='func'></me-tree>
  </li>
</ul>
</template>
<script>
import Vue from 'vue';
export default {
    name: 'meTree',
    data () {
        return {
            childrenShow: false

        };
    },
    props: {
        treeData: {
            default () {
                return [];
            }
        },
        addCount: {
            type: Number,
            default: 0
        }
    },
    computed: {
        count () {
            var newCount = this.addCount;
            return ++newCount;
        },
        treeStyle () {
            return {
                'padding-left': 20 * this.count + 'px'
            };
        }
    },
    methods: {
        openTree (item) {
            if (item.children) {
                if (!item.childrenShow) {
                    Vue.set(item, 'childrenShow', true);
                } else {
                    Vue.set(item, 'childrenShow', false);
                }
            }
        },
        treeItemCallBack (item) {
            if (!item.children) {
                let data = {
                    treeId: item.id,
                    treeName: item.title
                };
                this.$emit('toParent', data);
            }
        },
        func (data) {
            this.$emit('toParent', data);
        }
    },
    created () {
        console.log(this.treeData);
    }
};
</script>
<style type="text/css">
#meTree{
    width:100%;
}
#meTree li{
    list-style:none;
    text-align:left;
}
#meTree .tree-item{
    padding:4px;
}
#meTree .tree-item span{
    display: inline-block;
}
ul,li{
    padding:0;margin:0;
}
#meTree .tree-icon{
    padding:2px 4px;
    border:1px solid #ccc;
    margin-right:4px;
    cursor:pointer;
}
.tree-item{
    cursor:pointer;
}
</style>
