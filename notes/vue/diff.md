### vue 的 diff 算法

渲染真实 DOM 的开销很大，比如我们修改了某个数据，直接渲染到真实的 DOM 上，会引起整个 DOM 树的重排与重绘，影响性能，diff 算法可以帮助我们改善这种情况，在 vue 中，会根据真实 DOM 树生成一个 virtual DOM（Vnode, Object），当 virtual DOM 的某个节点发生改变后，会生成一个新的 Vnode，然后 Vnode 与 oldVnode 进行比较，发现有不一样的地方就直接修改在真实 DOM 上，然后将 oldVnode 赋值为 Vnode。diff 的过程就是调用 patch 函数，比较新旧节点，一边比较一边改变真实 DOM。

**Vnode 与 oldVnode 都是对象**

vue 中比较 Vnode 与 oldVnode 的差异时，采用同层级比较（react 同），以减小时间复杂度 O

### 分析

#### patch

```
function patch (oldVnode, vnode) {
    // some code
    if (sameVnode(oldVnode, vnode)) {
        patchVnode(oldVnode, vnode)
    } else {
        const oEl = oldVnode.el // 当前oldVnode对应的真实元素节点
        let parentEle = api.parentNode(oEl)  // 父元素
        createEle(vnode)  // 根据Vnode生成新元素
        if (parentEle !== null) {
            api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl)) // 将新元素添加进父元素
            api.removeChild(parentEle, oldVnode.el)  // 移除以前的旧元素节点
            oldVnode = null
        }
    }
    // some code 
    return vnode
}
```

patch 函数接收两个参数，oldVnode 与 Vnode，首先会比较两课树的根节点是否相同，以确定是否需要继续深入比较

#### sameNode
```
function sameVnode (a, b) {
  return (
    a.key === b.key &&  // key值
    a.tag === b.tag &&  // 标签名
    a.isComment === b.isComment &&  // 是否为注释节点
    // 是否都定义了data，data包含一些具体信息，例如onclick , style
    isDef(a.data) === isDef(b.data) &&  
    sameInputType(a, b) // 当标签是<input>的时候，type必须相同
  )
}
```

若跟节点不同，则认为 Vnode 完全改变了，直接替换 oldVnode，若相同，则调用 patchVnode 继续比较

#### patchNode
```
patchVnode (oldVnode, vnode) {
    const el = vnode.el = oldVnode.el
    let i, oldCh = oldVnode.children, ch = vnode.children
    if (oldVnode === vnode) return
    if (oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text) {
        api.setTextContent(el, vnode.text)
    }else {
        updateEle(el, vnode, oldVnode)
        if (oldCh && ch && oldCh !== ch) {
            updateChildren(el, oldCh, ch)
        }else if (ch){
            createEle(vnode) //create el's children dom
        }else if (oldCh){
            api.removeChildren(el)
        }
    }
}
```

* 找到对应的真实 dom，称为 el
* 判断 Vnode 和 oldVnode 是否指向同一个对象，如果是，那么直接 return，没有则继续
* 如果他们都有文本节点并且不相等，那么将 el 的文本节点设置为 Vnode 的文本节点
* 如果 oldVnode 有子节点而 Vnode 没有，则删除 el 的子节点
* 如果 oldVnode 没有子节点而 Vnode 有，则将 Vnode 的子节点真实化之后添加到 el
* 如果两者都有子节点，则执行 updateChildren 函数比较子节点

#### updateChildren

```
updateChildren (parentElm, oldCh, newCh) {
    let oldStartIdx = 0, newStartIdx = 0
    let oldEndIdx = oldCh.length - 1
    let oldStartVnode = oldCh[0]
    let oldEndVnode = oldCh[oldEndIdx]
    let newEndIdx = newCh.length - 1
    let newStartVnode = newCh[0]
    let newEndVnode = newCh[newEndIdx]
    let oldKeyToIdx
    let idxInOld
    let elmToMove
    let before
    while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
        if (oldStartVnode == null) {   // 对于vnode.key的比较，会把oldVnode = null
            oldStartVnode = oldCh[++oldStartIdx] 
        }else if (oldEndVnode == null) {
            oldEndVnode = oldCh[--oldEndIdx]
        }else if (newStartVnode == null) {
            newStartVnode = newCh[++newStartIdx]
        }else if (newEndVnode == null) {
            newEndVnode = newCh[--newEndIdx]
        }else if (sameVnode(oldStartVnode, newStartVnode)) {
            patchVnode(oldStartVnode, newStartVnode)
            oldStartVnode = oldCh[++oldStartIdx]
            newStartVnode = newCh[++newStartIdx]
        }else if (sameVnode(oldEndVnode, newEndVnode)) {
            patchVnode(oldEndVnode, newEndVnode)
            oldEndVnode = oldCh[--oldEndIdx]
            newEndVnode = newCh[--newEndIdx]
        }else if (sameVnode(oldStartVnode, newEndVnode)) {
            patchVnode(oldStartVnode, newEndVnode)
            api.insertBefore(parentElm, oldStartVnode.el, api.nextSibling(oldEndVnode.el))
            oldStartVnode = oldCh[++oldStartIdx]
            newEndVnode = newCh[--newEndIdx]
        }else if (sameVnode(oldEndVnode, newStartVnode)) {
            patchVnode(oldEndVnode, newStartVnode)
            api.insertBefore(parentElm, oldEndVnode.el, oldStartVnode.el)
            oldEndVnode = oldCh[--oldEndIdx]
            newStartVnode = newCh[++newStartIdx]
        }else {
           // 使用key时的比较
            if (oldKeyToIdx === undefined) {
                oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx) // 有key生成index表
            }
            idxInOld = oldKeyToIdx[newStartVnode.key]
            if (!idxInOld) {
                api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
                newStartVnode = newCh[++newStartIdx]
            }
            else {
                elmToMove = oldCh[idxInOld]
                if (elmToMove.sel !== newStartVnode.sel) {
                    api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
                }else {
                    patchVnode(elmToMove, newStartVnode)
                    oldCh[idxInOld] = null
                    api.insertBefore(parentElm, elmToMove.el, oldStartVnode.el)
                }
                newStartVnode = newCh[++newStartIdx]
            }
        }
    }
    if (oldStartIdx > oldEndIdx) {
        before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].el
        addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx)
    }else if (newStartIdx > newEndIdx) {
        removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx)
    }
}
```

* 将 Vnode 的子节点 Vch 和 oldVnode 的子节点 oldCh 提取出来
* oldCh 和 vCh 各有两个头尾的变量（类似指针）即 oldCh 上的 oldStartIdx、oldEndIdx 与 vCh 上的 newStartIdx、newEndIdx
* 两组会使用 sameNode 互相比较，一共有4种比较情况，sameNode(oldS, newS)、sameNode(oldE, newE)、sameNode(oldS, newE)、sameNode(oldE, newS)
* 若 oldS 和 newS 或 oldE 和 newE 匹配上了，不动 DOM，匹配上的两个指针向中间移动
* 若是 oldS 和 newE 匹配上了，那么真实 DOM 中的第一个节点会移到最后，匹配上的两个指针向中间移动
* 若是 oldE 和 newS 匹配上了，那么真实 DOM 中的最后一个节点会移到最前，匹配上的两个指针向中间移动
* 若四种匹配没有一对是成功的，那么遍历 oldChild，newS 挨个和他们匹配，匹配成功就在真实 DOM 中将成功的节点移到最前面，如果依旧没有成功的，那么将 newS 对应的节点插入到 DOM 中对应的 oldS 位置，oldS 和 newS 指针向中间移动
* 一旦 old 或 new 的 StartIdx > EndIdx 表明 oldCh和 vCh 至少有一个已经遍历完了，就会结束比较
* 若 oldS > oldE 表示 oldCh 先遍历完，那么就将多余的 vCh 根据 index 添加到 DOM 中
* 若 newS > newE表示 vCh 先遍历完，那么就在真实 DOM 中将区间为 [oldS, oldE] 的多余节点删掉
