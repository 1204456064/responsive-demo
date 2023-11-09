<template>
    <div></div>
</template>
<script setup lang="ts">
import { onMounted } from 'vue';
// =========================================== 纯编译时 ========================================
/**
 * 数据对象
 */
const compileDom = {
    tag: 'div',
    children: 'hello world',
};

console.log(compileDom);

// =========================================== 纯运行时 ========================================
/**
 * 数据对象
 */
const obj = {
    tag: 'div',
    props: {
        onClick: () => alert('hello'),
    },
    children: 'click me',
};

/**
 * 渲染函数
 * @param obj 数据对象
 * @param root 需要挂载的节点
 */
function renderDom(obj, root) {
    const el = document.createElement(obj.tag);

    if (typeof obj.children === 'string') {
        el.appendChild(document.createTextNode(obj.children));
    } else if (Array.isArray(obj.children)) {
        obj.children.forEach((child) => renderDom(child, el));
    }

    // 将元素添加到root节点下
    root.appendChild(el);
}

// ================================================= 创建虚拟节点
const vnode = {
    tag: 'div',
    props: {
        onClick: () => alert('hello'),
    },
    children: 'click me',
};

onMounted(() => {
    render(vnode, document.getElementById('a'));
});

/**
 *
 * @param obj 数据
 * @param container 需要
 */
function render(vnode, container) {
    const el = document.createElement(vnode.tag);

    for (const key in vnode.props) {
        if (/^on/.test(key)) {
            el.addEventListener(key.substring(2).toLowerCase(), vnode.props[key]);
        }
    }

    if (typeof vnode.children === 'string') {
        el.appendChild(document.createTextNode(vnode.children));
    } else if (Array.isArray(vnode.children)) {
        vnode.children.forEach((child) => render(child, el));
    }

    container.appendChild(el);
}
</script>
