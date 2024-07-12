---
title: React Hooks 库的探索与思考
class: text-center
layout: cover
background: https://proxy.viki.moe/photo-1456362150245-7f7d7ed8ebae?proxy-host=images.unsplash.com
# background: https://cover.sli.dev/
defaults:
  highlighter: shiki
  transition: fade-out
---

# React Hooks 库的探索与思考

👷 @Viki · 📅 2024.07

<div class="flex justify-center mt-16 mb-8" >
  <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-24" />
</div>

<code><a href="https://sheinsight.github.io/react-use">@shined/react-use</a></code> v1 正式版已发布！🎉

---
layout: center
---

<div class='text-center'>
  <mdi-react class='my-4 size-16' />
</div>

# React 的重要事件节点

---

<v-clicks>

- 2013 年 05 月 —— React 正式开源 (v0.7)

- 2015 年 07 月 —— React Native 发布

- 2016 年 04 月 —— React 跃升大版本 (v0.14 => v15)

- 2016 年 07 月 —— React Fiber 发布 (v16)

- 2018 年 02 月 —— React Context API 发布 (v16.3)

- **<span v-mark="{ at: 11, color:'#f59e0b', type: 'underline' }">2018 年 10 月 —— React Hooks 提案</span>**

- **<span v-mark="{ at: 11, color:'#f59e0b', type: 'underline' }">2019 年 02 月 —— React Hooks 正式发布 (v16.8)</span>**

- 2020 年 05 月 —— React JSX-Runtime (v17)

- 2022 年 06 月 —— React Concurrent Mode & Batch Update (v18)

- 2024 年 04 月 —— React Server Component 发布 (v19 RC)

</v-clicks>
...

---
layout: center
---

# Function Component + Hooks 成为主流

---

## Class Component

```tsx twoslash
// === before ===
import React, { Component } from 'react'
class Counter extends Component {
  state = { count: 0 }
  render() {
    return (
      <button onClick={() => this.setState({ count: this.state.count + 1 })}>
        {this.state.count}
      </button>
    )
  }
}
```

<br />

<v-click>

## Function Component

```tsx twoslash
// === after ===
import React, { useState } from 'react'
function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```

</v-click>

---

# Hooks 带来的变革

<v-clicks>

- 函数组件拥有了此前只有类组件才有的能力
  - 状态管理 (useState)
  - 生命周期 (useEffect)
  - 上下文 (useContext)
  - ...

- 提高了开发体验
  - 改善了代码的组织和复用性
  - 使得代码逻辑更易于理解
  - 使得代码更容易测试
  - ...

</v-clicks>

---

# 社区涌现出大批优秀的 Hooks 库

## Hooks 集合

- <a href='https://github.com/streamich/react-use'><mdi-github /> streamich/react-use (react-use)</a>
- <a href='https://github.com/alibaba/hooks'><mdi-github /> alibaba/hooks (ahooks)</a>
- <a href='https://github.com/antonioru/beautiful-react-hooks'><mdi-github /> antonioru/beautiful-react-hooks</a>
- <a href='https://github.com/uidotdev/usehooks'><mdi-github /> uidotdev/usehooks</a>
- ...

## 特定功能 Hooks

- <a href='https://github.com/vercel/swr'><mdi-github /> vercel/swr (useSWR)</a>
- <a href='https://github.com/tannerlinsley/react-query'><mdi-github /> tannerlinsley/react-query (useQuery)</a>
- <a href='https://github.com/tannerlinsley/reduxjs/react-redux'><mdi-github /> reduxjs/react-redux (useSelector)</a>
- <a href='https://github.com/tannerlinsley/immerjs/use-immer'><mdi-github /> immerjs/use-immer (useImmer)</a>
- ...

---
layout: two-cols
---

# 一个简单的 Counter 组件

````md magic-move {at:2}
```tsx {*|2-4|6-10|*|2-4}
function Counter() {
  const [count, setCount] = useState(0)
  const add = () => setCount(count + 1)
  const minus = () => setCount(count - 1)
  return (
    <div>
      <button onClick={add}>-</button>
      <span>{count}</span>
      <button onClick={minus}>+</button>
    </div>
  )
}
```

```tsx {*|}
function useCounter(initialCount = 0) {
  const [count, setCount] = useState(initialCount)
  const add = () => setCount(count + 1)
  const minus = () => setCount(count - 1)
  return { count, add, minus }
}
```

```tsx
function Counter() {
  const { count, add, minus } = useCounter(0)
  return (
    <div>
      <button onClick={add}>-</button>
      <span>{count}</span>
      <button onClick={minus}>+</button>
    </div>
  )
}
```

````

::right::

<div class='text-center my-12' v-click="1">React 组件 = 业务逻辑 + UI 界面</div>

<div class='text-center my-12 flex gap-12 justify-center'>
  <span v-click="2">业务逻辑</span><span v-click="3">UI 界面</span>
</div>

<div class='text-center my-12 flex gap-4 justify-center'>
  <span v-click="7">Hooks</span><span v-click="8">👉</span><span v-click="8">逻辑组件</span>
</div>

<div class='text-center my-12' v-click="9">React 组件 = 逻辑组件 (Hooks) + UI 界面</div>

---
layout: center
---

# useCounter 的打怪升级之路

---

# useCounter 的打怪升级之路

<div class='mb-8'>
  <div v-if="$clicks === 0">✅ 运行良好</div>
  <div v-if="$clicks === 1">🆕 新需求：首次 +1，此后每间隔固定时间 +1</div>
  <div v-if="$clicks === 2">😨 组件死循环了，为什么？</div>
  <div v-if="$clicks === 3">🐛 罪魁祸首：<code>add</code> 函数每次渲染都会重新创建</div>
  <div v-if="$clicks === 4">😎 小意思，上 <code>useMemoizedFn</code></div>
  <div v-if="$clicks === 5">😄 效果显著，完美解决</div>
  <div v-if="$clicks === 6">🆕 新场景：用户交互触发异步操作，但完成前用户离开了</div>
  <div v-if="$clicks === 7">🆕 新场景：但后续存在 <code>setState</code> 操作 (即组件卸载后)</div>
  <div v-if="[8, 9].includes($clicks)">🤔️ 虽然这次只是个 Warning，不影响功能，但是还得处理</div>
  <div v-if="$clicks === 10">🙋 这题我会，该 <code>useSafeState</code> 出马了</div>
  <div v-if="$clicks === 11">🤗 这下就没问题了，组件卸载后永远也不会执行了 (<a href="https://github.com/alibaba/hooks/blob/b2f12185963b7efe46ec70c97f661849f89892b5/packages/hooks/src/useSafeState/index.ts#L13-L15">按照 ahooks 的逻辑</a>)</div>
  <div v-if="$clicks === 12">🆙 新需求：需支持重置操作，同时支持传参改变初始值</div>
  <div v-if="$clicks === 13">🔄 防止 <code>useMemoizedFn</code> 实现不规范，未读取最新函数</div>
  <div v-if="$clicks === 14">🔄 使用 <code>setState((state) => newState)</code> 的写法</div>
  <div v-if="$clicks === 15">🤔️ 那，其他状态怎么办？</div>
  <div v-if="$clicks === 16">🪄 <code>useLatest</code> 派上用场</div>
  <div v-if="$clicks === 17">🤌 收拢状态和操作，统一返回风格</div>
  <div v-if="$clicks === 18">📌 <code>actions</code> 对象也需要保持引用稳定，同时优化参数和命名</div>
  <div v-if="$clicks === 19">🥳 一个成熟的 <code>useCounter</code> 大功告成！</div>
  <div v-if="$clicks === 20">🔍 虽然看着没什么变化，实则内部存在大量优化</div>
  <div v-if="$clicks === 21">🫣 这基本就是 <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-6 inline" /> <code>@shined/react-use</code> 库里的 <code>useCounter</code> 的核心</div>
  <div v-if="$clicks === 22">22</div>
</div>

````md magic-move
```tsx
function Counter() {
  const { count, add, minus } = useCounter(0)
  return (
    <div>
      <button onClick={add}>-</button>
      <span>{count}</span>
      <button onClick={minus}>+</button>
    </div>
  )
}
```

```tsx {*|7}
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  useEffect(() => {
    add()
    const timer = setInterval(add, 1000);
    return () => clearInterval(timer) 
  }, [add])
  return <div>OtherComponent</div>
}
```

```tsx {3}
function useCounter(initialCount = 0) {
  const [count, setCount] = useState(initialCount)
  const add = () => setCount(count + 1)
  const minus = () => setCount(count - 1)
  return { count, add, minus }
}
```

```tsx {3,4}
function useCounter(initialCount = 0) {
  const [count, setCount] = useState(initialCount)
  const add = useMemoizedFn(() => setCount(count + 1))
  const minus = useMemoizedFn(() => setCount(count - 1))
  return { count, add, minus }
}
```

```tsx
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  useEffect(() => {
    add()
    const timer = setInterval(add, 1000);
    return () => clearInterval(timer) 
  }, [add])
  return <div>OtherComponent</div>
}
```

```tsx
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  async function handleClick() {
    await fetchData(); // 3000ms
    add() 
  }
  return <button onClick={handleClick}>{count}</button>
}
```

```tsx
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  async function handleClick() {
    await fetchData(); // 3000ms
    // When component unmount, `setState` will still be called.
    add() 
  }
  return <button onClick={handleClick}>{count}</button>
}
```

```tsx
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  async function handleClick() {
    await fetchData(); // 3000ms
    // Warning: Can't perform a React state update on an unmounted component. This is a no-op,
    // but it indicates a memory leak in your application.
    // To fix, cancel all subscriptions and asynchronous tasks in a useEffect cleanup function.
    add() 
  }
  return <button onClick={handleClick}>{count}</button>
}
```

```tsx
function useCounter(initialCount = 0) {
  const [count, setCount] = useState(initialCount)
  const add = useMemoizedFn(() => setCount(count + 1))
  const minus = useMemoizedFn(() => setCount(count - 1))
  return { count, add, minus }
}
```

```tsx {2|*}
function useCounter(initialCount = 0) {
  const [count, setCount] = useSafeState(initialCount)
  const add = useMemoizedFn(() => setCount(count + 1))
  const minus = useMemoizedFn(() => setCount(count - 1))
  return { count, add, minus }
}
```

```tsx {2,6-14}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const add = useMemoizedFn(() => setCount(count + 1))
  const minus = useMemoizedFn(() => setCount(count - 1))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(initial)
    }
  })
  return { count, add, minus, reset }
}
```

```tsx {4,5}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const add = useMemoizedFn(() => setCount(count + 1))
  const minus = useMemoizedFn(() => setCount(count - 1))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(initial)
    }
  })
  return { count, add, minus, reset }
}
```

```tsx {4,5}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const add = useMemoizedFn(() => setCount(count => count + 1))
  const minus = useMemoizedFn(() => setCount(count => count - 1))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(initial)
    }
  })
  return { count, add, minus, reset }
}
```

```tsx {11}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const add = useMemoizedFn(() => setCount(count => count + 1))
  const minus = useMemoizedFn(() => setCount(count => count - 1))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(initial) // What about here?
    }
  })
  return { count, add, minus, reset }
}
```

```tsx {4,12}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const latest = useLatest({ initial })
  const add = useMemoizedFn(() => setCount(count => count + 1))
  const minus = useMemoizedFn(() => setCount(count => count - 1))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(latest.current.initial)
    }
  })
  return { count, add, minus, reset }
}
```

```tsx {15,16}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const latest = useLatest({ initial })
  const add = useMemoizedFn(() => setCount(count => count + 1))
  const minus = useMemoizedFn(() => setCount(count => count - 1))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(latest.current.initial)
    }
  })
  const actions = { add, minus, reset }
  return [count, actions]
}
```

```tsx {5,6,15,16|*}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const latest = useLatest({ initial })
  const inc = useMemoizedFn((delta = 1) => setCount(count => count + delta))
  const dec = useMemoizedFn((delta = 1) => setCount(count => count - delta))
  const reset = useMemoizedFn((newInitial) => {
    if (newInitial !== undefined) {
      setInitial(newInitial)
      setCount(newInitial)
    } else {
      setCount(latest.current.initial)
    }
  })
  const actions = useCreation(() => ({ inc, dec, reset }), [])
  return [count, actions]
}
```

```tsx
function Counter() {
  const [count, actions] = useCounter(0)
  return (
    <div>
      <button onClick={() => actions.inc(1)}>-</button>
      <span>{count}</span>
      <button onClick={() => actions.dec(1)}>+</button>
    </div>
  )
}
```

```tsx {1,3}
import { useCounter } from '@shined/react-use'
function Counter() {
  const [count, actions] = useCounter(0)
  return (
    <div>
      <button onClick={() => actions.inc(1)}>-</button>
      <span>{count}</span>
      <button onClick={() => actions.dec(1)}>+</button>
    </div>
  )
}
```
````

---
layout: center
---

<div class="flex justify-center mb-12" >
  <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-36" />
</div>

<div class='text-center mb-8'>
  <code class='text-3xl!'>@shined/react-use</code>
</div>

<v-click>
一个 SSR 友好、全面、标准化和高度优化的 React Hooks 库
</v-click>


---

<div>
  <div class="flex flex-col justify-center my-8" >
    <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-24 mb-4" />
    <div class='text-center'><code>@shined/react-use</code></div>
  </div>
  <div class='size-full text-2xl flex flex-col justify-center items-center'>
    <div class='flex gap-6 my-6'>
      <div class='w-48 group'>
        <mdi-sparkles-outline class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Animation
      </div>
      <div class='w-48 group'>
        <mdi-google-chrome class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Browser
      </div>
      <div class='w-48 group'>
        <mdi-hexagon-slice-6 class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Element
      </div>
    </div>
    <div class='flex gap-6 my-6'>
      <div class='w-48 group'>
        <mdi-react class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Lifecycle
      </div>
      <div class='w-48 group'>
        <mdi-internet class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Network
      </div>
      <div class='w-48 group'>
        <mdi-toolbox-outline class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />ProUtilities
      </div>
    </div>
    <div class='flex gap-6 my-6'>
      <div class='w-48 group'>
        <mdi-access-point class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Sensors
      </div>
      <div class='w-48 group'>
        <mdi-atom class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />State
      </div>
      <div class='w-48 group'>
        <mdi-tools class='mx-2 text-[#2e8555]/80 group-hover:text-amber/80' />Utilities
      </div>
    </div>
  </div>
</div>
---
layout: center
---

# @shined/react-use 的特性与内部优化

---

# 全面

- 划分了 9 个大类，涵盖 Web 开发的各个方面
- 迄今为止已经有 100+ 个 Hooks，仍在增加中

<img v-click class='h-48 mt-8 rounded-md' src='/categories.png' />

<tip v-click>不管开发什么系统，总能找到你用得到的 <b>Hooks</b></tip>

---

# 轻量

- 真正 0 依赖，减轻项目体积
- 采用 ESM 设计和交付，支持 <code>Tree Shaking</code>，无副作用

<img v-click class='h-48 mt-8 rounded-md' src='/zero-dependencies.png' />

<tip v-click>要用什么 <b>Hooks</b>，你只管引入就好</tip>

---

# 灵活

- 支持 <code>ElementTarget</code>、<code>Pausable</code>、<code>Ref Getter</code> 等特性。

<div v-click class='mt-4'>

```tsx twoslash
import { useTargetElement, useElementSize, usePausable, useMouse } from '@shined/react-use'

const el = useTargetElement("#target")

const size = useElementSize(() => document.documentElement)

const pausable = usePausable(false, () => {}, () => {});
const { isActive, pause, resume } = pausable;

const { x, y } = useMouse();
// const { x, y, isActive, pause, resume } = useMouse();

if (isActive()) {
  // do something...
}
```

</div>

<tip v-click>所有 <b>Target</b> 参数都经过 <b>useTargetElement</b> 处理，确保开发体验一致性</tip>

---

# 高度优化 + 最佳实践

<v-click at="1">
  <code>Safe State</code>、<code>Latest Value</code>、<code>Stabilization</code> 等。
</v-click>

<div v-click class='mt-8'>

```tsx
const [count, setCount] = useSafeState(0)

const latest = useLatest({ initial })

const add = useStableFn((delta = 1) => {
  setState(count => count + delta)
})
```

</div>

<tip v-click>内部所有 <b>Hooks</b> 均严格遵循以上优化，提供最佳实践</tip>

---

# 多环境友好

<v-click at="1">
  支持 SSR (<code>Next.js</code>)、<code>React Native</code>、<code>Ink</code> 等。
</v-click>

<div v-click class='mt-8'>

```tsx
import { render, Text } from "ink";
import { useIntervalFn, useCounter } from '@shined/react-use';

function App() {
  const [count, actions] = useCounter(0)
  useIntervalFn(actions.inc, 1000)
  return <Text>Count: {count}</Text>
}

render(<App />);
```

</div>

<tip v-click>只要能用 <b>useEffect</b> 的地方，就能用 <b>@shined/react-use</b></tip>

---
layout: iframe-right
url: https://sheinsight.github.io/react-use/
---

# 交互式文档

- 高质量的上手指南
- 中英文双语支持
- 交互式在线 Demo
- 规范化的 API 说明
- 源码一站式直达

<img src="/doc-well-group.png" class='mt-4 w-300px rounded' v-click />

---

# 出色的开发体验 (DX)

<v-click at="1">

- 全面使用 <code>TypeScript</code>
- 辅以 <code>JSDoc</code> 注释
- 提倡“代码即文档”，减少上下文切换

</v-click>

<div v-click="1" class='mt-8'>

```tsx twoslash
import { useBluetooth } from '@shined/react-use'

function App() {
  const bluetooth = useBluetooth({
    acceptAllDevices: true,
    optionalServices: ['battery_service', 'device_information'],
    immediate: false,
  })
}
```

<!-- <v-click at="1">

<div class='flex flex-col gap-4'>
  <img class='h-48 mt-8 rounded-md' src='/code-is-doc-1.png' />
  <img class='h-48 mt-8 rounded-md' src='/code-is-doc-2.png' />
</div>

</v-click>  -->

</div>

---
layout: center
---

# @shined/react-use 的现代化配置

---
layout: two-cols
---

<div class='flex flex-col gap-12 mt-12'>
  <v-clicks>
  <div class='flex items-center'>
    <span>仓库结构：</span>
    <vscode-icons-file-type-light-pnpm class="size-8 mx-2" /> pnpm + 
    <vscode-icons-file-type-git class="size-8 mx-2" /> Monorepo
  </div>
  <div class='flex items-center'>
    <span>版本锁定：</span>
    <vscode-icons-file-type-node class="size-8 mx-2" /> .node-version + 
    <vscode-icons-folder-type-package class="size-8 mx-2" /> pkgMgr
  </div>
  <div class='flex items-center'>
    <span>格式化和规范检查：</span>
    <vscode-icons-file-type-biome class="size-8 mx-2" /> Biome +
    <img src="/oxlint.svg" class="inline size-8 mx-2" /> oxlint
  </div>
  <div class='flex items-center'>
    <span>文档与示例：</span>
    <img src="/docusaurus.svg" class="inline size-8 mx-2" /> Docusaurus + 
    <vscode-icons-file-type-mdx class="size-8 mx-2" /> MDX
  </div>
  <div class='flex items-center'>
    <span>Demo 布局与样式：</span>
    <vscode-icons-file-type-unocss class="size-8 mx-2" /> UnoCSS
  </div>
  </v-clicks>
</div>

::right::

<div class='flex flex-col gap-12 mt-12'>
  <v-clicks>
    <div class='flex items-center'>
    <span>测试与打包：</span>
    <vscode-icons-file-type-vitest class="size-8 mx-2" /> Vitest +
    <vscode-icons-file-type-esbuild class="size-8 mx-2" /> tsup (esbuild)
  </div>
  <div class='flex items-center'>
    <span>代码质量保障：</span>
    <vscode-icons-folder-type-husky class="size-8 mx-2" /> Husky + 
    <img src="/lint-staged.png" class="inline size-8 mx-2" /> Lint Staged
  </div>
  <div class='flex items-center'>
    <span>约定式提交：</span>
    <img src="/conventional-commit.png" class="rounded inline size-8 mx-2" /> Conventional Commit
  </div>
  <div class='flex items-center'>
    <span>发布：</span>
    <vscode-icons-folder-type-github class="size-8 mx-2" /> GitHub Release +
    <img src="/changelog.svg" class="inline size-8 mx-2" /> <code>CHANGELOG.md</code>
  </div>
  <div class='flex items-center'>
    <span>供应链安全：</span>
    <img src="/github-action.svg" class="inline size-8 mx-2" /> Action + 
    <mdi-verified class="text-[#327532] size-8 mx-2" /> npm Provenance
  </div>
  </v-clicks>
</div>

---
layout: center
---

# 一些思考

---

# State vs Ref

State 和 Ref 都可以用来保存信息，该如何抉择

- 全局变量对于所有组件实例都是共享的，导致组件之间的状态混乱
- 渲染阶段会在每次更新时被执行，let 和 const 会被重新声明
- 遵循组件生命周期且多次渲染仍稳定存在的“状态”：State 和 Ref
- State 的更改会触发组件的重新渲染，而 Ref 的更改则不会

```tsx
// 只会被声明一次，但会被所有组件实例共享
let globalCount = 0 

function Counter() {
  // 每次渲染都会被重新声明
  let count = 0 
  // 只会被声明一次，更改不会触发重新渲染
  const countRef = useRef(0) 
  // 只会被声明一次，更改会触发重新渲染
  const [count, setCount] = useState(0) 
  // ...
}
```

---

# State

- 用于保留渲染之间数据的状态变量 (渲染稳定)。
- 状态对于组件的每个副本都是本地的 (实例隔离)。
- 更改它会触发重新渲染。

<br />

# Ref

- 可以在重新渲染之间存储信息 (渲染稳定，不像局部变量)。
- 该信息对于组件的每个副本都是本地的 (实例隔离)。
- 更改它不会触发重新渲染 (不触发渲染，与 State 不同)。

---

# 引入 Ref Getter

兼具信息存储功能与渲染优化的取舍与妥协

```ts
// const pausable = usePausable(); // { isActive, resume, pause }

const { isActive, resume, pause } = useIntervalFn(() => {
  // do something
}, 1000)

function handleClick() {
  if (isActive()) {
    // do something when active
  }
}
```

<tip v-click><b>isActive</b> 状态的变更，默认不会触发重新渲染，值可通过 Getter 函数获取</tip>

---

# isActive (Ref Getter) 的内部

<br />

```ts
const isActive = () => ref.current
```

<br />

<div v-click>这就结束了？</div>

<br />

<div v-click>

```tsx
const isActive = useStableFn(() => ref.current)
```

</div>

<br />

<div v-click>

```tsx
const [isActiveRef, isActive] = useGetterRef(false)
```

</div>

<tip v-click>利用 <b>useGetterRef</b> 来创建 Ref Getter，底层使用 Ref 而不是 State</tip>

---
layout: center
---

# useSafeState 做了什么

---

# 问题背景

- 组件卸载后执行 setState，React 会抛出警告
- 本质上，组件卸载后执行的 setState 只是一个 noop (空函数)
- 抛出的警告并不意味着内存泄漏，只是在提醒你清理定时器、订阅函数等可能导致内存泄漏的操作
- React 官方意识到了这个警告的不准确，从 React 18 开始移除了它
- 官方指出，组件卸载后应当执行 setState，未来可能会有「状态保持」等特性依赖于此
- 这个时候卸载后的 setState 将有意义，换句话说，卸载后不执行 setState 将会导致更严重的问题
- 因而在没有抛出警告时，不应当拦截 setState 操作（尽管目前而言只是 noop，没有实际意义）
- 更多讨论可以参考 [React Discussion #82](https://github.com/reactwg/react-18/discussions/82)

<img src="/set-state.png" class='mt-4 h-160px rounded' v-click />

---

## ahooks 的做法

- 组件卸载后，不执行 setState，以抑制抛出的警告
- 其余与 useState 一致

## @shined/react-use 的做法

- React 18 及以上版本，等同于 useState，不做任何处理
- React 17 及以下版本，组件卸载后，不执行 setState，以抑制抛出的警告
- 新增了可选的 deep 选项，用于深度对比并决定是否更新，默认 false，具备渲染优化

<br />

<v-click>

```tsx
const [value, setValue] = useSafeState({ count: 1 }, { deep: true })

setValue({ count: 1}) // “同样”的对象，如果未设置 deep，将触发组件重新更新
```

</v-click>

<tip v-click>在状态「简单、可控」时，一次 deep compare 的开销远比一次组件渲染小的多得多</tip>

---
layout: center
---

# 单一职责

---

# 一次只做一件事，并把它做好

---
layout: center
---

# 关于 React 19

---

---
layout: center
---

# Hooks 使用引导 / 高频预测

---

# 常用 Hooks

<v-clicks>

- **生命周期**：useMount / useUnmount / useEffectOnce / useUpdateEffect
- **状态相关**：useSafeState / useSetState / useToggle / useCircularList
- **Loading 状态**：useAsyncFn (useLoadingFn) / createSingleLoading
- **节流/防抖/定时器**：useDebounceFn / useThrottleFn / useIntervalFn (支持 RAF)
- **强业务/场景化**：useInfiniteScroll / useMultiSelect / useDynamicList / useCountdown
- **时间格式化**：useDateFormat (dayjs、moment、date-fns 简单使用场景的平替)
- **复制操作 (剪贴板)**：useClipboard (读、写，自动降级到 <code>document.exec('copy')</code>)
- **Hooks 老手/上层封装**: useStableFn / useLatest / useCreation

</v-clicks>

---
layout: center
---

# @shined/react-use 的后续规划

---

# 后续规划

<v-clicks>

- 起步期（当前阶段）
  - 以原子化为主，也包含部分场景化 Hooks
  - 覆盖大部分场景，已生产可用 （目前 137 个）
- 业务强化期
  - 封装强业务的上层 Hooks
  - 做到 「常见场景，一行代码开箱即用」
- 生态完善期
  - 与 shineout 等主流库做集成联动
  - 简化业务开发，同时扩大生态

</v-clicks>

---
layout: center
---

# 回顾

---

# 回顾

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8

---
layout: end
---

# The End

感谢大家的耐心收听

## （Q&A）

