<template>
    <div class="try-h">
        it is try vue
    </div>
</template>

<script setup>
import { h } from 'vue';

function renderer(vnode, container) {
    if (typeof vnode.type === 'string') {
        // 说明 vnode 描述的是标签元素
        mountElement(vnode, container)
    } else if (typeof vnode.type === 'function') {
        // 说明 vnode 描述的是组件
        mountComponent(vnode, container)
    }
}

function mountElement(vnode, container) {
    // 使用 vnode.type 作为标签名称创建 DOM 元素
    const el = document.createElement(vnode.type)
    // 遍历 vnode.props 将属性、事件添加到 DOM 元素
    for (const key in vnode.props) {
        if (/^on/.test(key)) {
            // 如果 key 以 on 开头，那么说明它是事件
            el.addEventListener(
                key.substr(2).toLowerCase(), // 事件名称 onClick ---> click
                vnode.props[key] // 事件处理函数
            )
        }
    }

    // 处理 children
    if (typeof vnode.children === 'string') {
        // 如果 children 是字符串，说明是元素的文本子节点
        el.appendChild(document.createTextNode(vnode.children))
    } else if (Array.isArray(vnode.children)) {
        // 递归地调用 renderer 函数渲染子节点，使用当前元素 el 作为挂载点
        vnode.children.forEach(child => renderer(child, el))
    }

    // 将元素添加到挂载点下
    container.appendChild(el)
}

function mountComponent(vnode, container) {
    // 调用组件函数，获取组件要渲染的内容（虚拟 DOM）
    const subtree = vnode.type()
    // 递归调用 renderer 渲染 subtree
    renderer(subtree, container)
}
// 组件是一组dom元素的封装
// 编译器 => 渲染器
const MyComponent = function () {
    const vNode = h('h1', { onClick: () => console.log('click') }, 'it is me, vnode element')
    console.log(vNode)
    return vNode;

    //   return {
    //     tag: 'div',
    //     props: {
    //       onClick: () => alert('hello')
    //     },
    //     children: 'click me'
    //   }
}

const vnode = {
    type: MyComponent
}

renderer(vnode, document.body)
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
