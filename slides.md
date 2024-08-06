```yaml
title: React Hooks 库的探索与思考
class: text-center
layout: cover
background: https://proxy.viki.moe/photo-1456362150245-7f7d7ed8ebae?proxy-host=images.unsplash.com
# background: https://cover.sli.dev/
defaults:
  highlighter: shiki
  transition: fade-out
```

# React Hooks 库的探索与思考

👷 @Viki · 📅 2024.08

<div class="flex justify-center mt-16 mb-8" >
  <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-24" />
</div>

<code><a href="https://sheinsight.github.io/react-use" class='text-dark dark:text-white'>@shined/react-use</a></code> v1 正式版已发布！🎉

---

```yaml
layout: center
```

<div class='text-center'>
  <mdi-react class='my-4 size-16' />
</div>

# React 的重要事件节点

---

# React 的重要事件节点

<v-clicks>

- 2013 年 05 月 —— React 正式开源 (v0.7)

- 2015 年 07 月 —— React Native 发布

- 2016 年 04 月 —— React 跃升大版本 (v0.14 => v15)

- 2016 年 07 月 —— React Fiber 发布 (v16)

- 2018 年 02 月 —— React Context API 发布 (v16.3)

- **<span v-mark="{ at: 11, color: '#f59e0b', type: 'underline' }">2018 年 10 月 —— React Hooks 提案</span>**

- **<span v-mark="{ at: 11, color: '#f59e0b', type: 'underline' }">2019 年 02 月 —— React Hooks 正式发布 (v16.8)</span>**

- 2020 年 05 月 —— React JSX-Runtime (v17)

- 2022 年 06 月 —— React Concurrent Mode & Batch Update (v18)

- 2024 年 04 月 —— React Server Component 发布 (v19 RC)

</v-clicks>

---

```yaml
layout: center
```

# 函数组件 (Function Component) + Hooks 成为主流

---

## 类组件 (Class Component)

```tsx
// === before ===
import React from 'react'
class Counter extends React.Component {
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

## 函数组件 (Function Component) + Hooks

```tsx
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

- 提高了开发体验 (DX)

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

```yaml
layout: center
```

# 「逻辑组件」

---

```yaml
layout: two-cols
```

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

```tsx {*|*}
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
  <span v-click="7">Hooks</span><span v-click="8">👉</span><span v-click="8">「逻辑组件」</span>
</div>

<div class='text-center my-12' v-click="9">React 组件 = 「逻辑组件」 (Hooks) + UI 界面</div>

---

```yaml
layout: center
```

# useCounter 的打怪升级之路

---

# useCounter 的打怪升级之路

<div class='mb-8'>
  <div v-if="$clicks === 0">✅ 现有的简单场景下，运行良好</div>
  <div v-if="$clicks === 1">🆕 新场景：首次 Mount +1，此后每间隔固定时间 +1</div>
  <div v-if="$clicks === 2">😨 组件死循环了，为什么？</div>
  <div v-if="$clicks === 3">🐛 罪魁祸首：<code>add</code> 函数每次渲染都会重新创建</div>
  <div v-if="$clicks === 4">😎 小意思，上 <code>useMemoizedFn</code></div>
  <div v-if="$clicks === 5">😄 效果显著，完美解决</div>
  <div v-if="$clicks === 6">🆕 新场景：用户交互触发异步操作，在完成前用户提前离开了</div>
  <div v-if="$clicks === 7">🆕 新场景：但后续存在 <code>setState</code> 操作 (即组件卸载后)</div>
  <div v-if="[8, 9].includes($clicks)">🤔️ 虽然没有问题，但是报了个 Warning （React <= v17）</div>
  <div v-if="$clicks === 10">🙋 这题我会，该 <code>useSafeState</code> 出马了</div>
  <div v-if="$clicks === 11">🤗 这下组件卸载后，永远也不会执行 <code>setState</code> 了 (<a href="https://github.com/alibaba/hooks/blob/b2f12185963b7efe46ec70c97f661849f89892b5/packages/hooks/src/useSafeState/index.ts#L13-L15">按照 ahooks 的逻辑</a>)</div>
  <div v-if="$clicks === 12">🆙 新需求：需支持重置操作，同时支持传参改变初始值</div>
  <div v-if="$clicks === 13">🔄 防止 <code>useMemoizedFn</code> 实现不规范，未读取最新函数</div>
  <div v-if="$clicks === 14">🔄 使用 <code>setState((state) => newState)</code> 的写法</div>
  <div v-if="$clicks === 15">🤔️ 那，其他状态怎么办？</div>
  <div v-if="$clicks === 16">🪄 <code>useLatest</code> 派上用场</div>
  <div v-if="$clicks === 17">🤌 收拢状态和操作，统一返回风格</div>
  <div v-if="$clicks === 18">📌 <code>actions</code> 对象也需要保持引用稳定，同时优化参数和命名</div>
  <div v-if="$clicks === 19">😇 继续完善 TypeScript 类型、JSDoc 注释，并统一命名</div>
  <div v-if="$clicks === 20">🥳 一个成熟的 <code>useCounter</code> 大功告成！</div>
  <div v-if="$clicks === 21">🔍 虽然使用起来没太多变化，但是其实内部已经进行了大量优化</div>
  <div v-if="$clicks === 22">🫣 这基本就是 <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-6 inline" /> <code>@shined/react-use</code> 里 <code>useCounter</code> 的核心</div>
  <div v-if="$clicks === 23">22</div>
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

```tsx {*|*}
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  useEffect(() => {
    add()
    const timer = setInterval(add, 1000)
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
    const timer = setInterval(add, 1000)
    return () => clearInterval(timer)
  }, [add])
  return <div>OtherComponent</div>
}
```

```tsx
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  async function handleClick() {
    await fetchData() // 3000ms
    add()
  }
  return <button onClick={handleClick}>{count}</button>
}
```

```tsx
function OtherComponent() {
  const { count, add, minus } = useCounter(0)
  async function handleClick() {
    await fetchData() // 3000ms
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
    await fetchData() // 3000ms
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
  const reset = useMemoizedFn(newInitial => {
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
  const reset = useMemoizedFn(newInitial => {
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
  const reset = useMemoizedFn(newInitial => {
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
  const reset = useMemoizedFn(newInitial => {
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
  const reset = useMemoizedFn(newInitial => {
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
  const reset = useMemoizedFn(newInitial => {
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

```tsx {5,6,15,16}
function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const latest = useLatest({ initial })
  const inc = useMemoizedFn((delta = 1) => setCount(count => count + delta))
  const dec = useMemoizedFn((delta = 1) => setCount(count => count - delta))
  const reset = useMemoizedFn(newInitial => {
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
export interface UseCounterReturnsAction {
  /**
   * increment the counter
   * @param {number} [delta=1] - The increment value
   */
  inc: (delta?: number) => void
  /**
   * decrement the counter
   * @param {number} [delta=1] - The decrement value
   */
  dec: (delta?: number) => void
  /**
   * reset the counter
   * @param {number} [n] - The reset value
   */
  reset: (n?: number) => void
}

export type Count = number
export type UseCounterReturns = readonly [Count, UseCounterReturnsAction]
```

```tsx
// TS types & JSDoc comments are omitted

function useCounter(initialCount = 0) {
  const [initial, setInitial] = useSafeState(initialCount)
  const [count, setCount] = useSafeState(initial)
  const latest = useLatest({ initial })
  const inc = useMemoizedFn((delta = 1) => setCount(count => count + delta))
  const dec = useMemoizedFn((delta = 1) => setCount(count => count - delta))
  const reset = useMemoizedFn(newInitial => {
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

```yaml
layout: center
```

<div class="flex justify-center mb-12" >
  <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-36" />
</div>

<div class='text-center mb-8'>
  <code class='text-3xl!'>@shined/react-use</code>
</div>

<v-click>
一个多环境 (SSR) 友好、全面、标准化和高度优化的 React Hooks 库
</v-click>

---

<feature />

---

```yaml
layout: center
```

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

const el = useTargetElement('#target')
const size = useElementSize(() => document.documentElement)
const pausable = usePausable(
  false,
  () => {},
  () => {}
)
const { isActive, pause, resume } = pausable
const { x, y } = useMouse()
// const { x, y, isActive, pause, resume } = useMouse();
if (isActive()) {
  // do something...
}
```

</div>

<tip v-click>所有 <b>Target</b> 参数都经过 <b>useTargetElement</b> 处理，确保开发体验一致性</tip>

---

# 高度优化

<v-click at="1">
包括但不限于 <code>Safe State</code>、<code>Latest Value</code>、<code>Stabilization</code> 等最佳实践。
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
import { render, Text } from 'ink'
import { useIntervalFn, useCounter } from '@shined/react-use'

function App() {
  const [count, actions] = useCounter(0)
  useIntervalFn(actions.inc, 1000)
  return <Text>Count: {count}</Text>
}

render(<App />)
```

</div>

<tip v-click>只要能用 <b>useEffect</b> 的地方，就能用 <b>@shined/react-use</b></tip>

---

# 出色的开发体验 (DX)

<v-click at="1">

- 全面使用 <code>TypeScript</code>，辅以 <code>JSDoc</code> 注释
- 提倡“代码即文档”，减少上下文切换干扰

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

```yaml
layout: center
```

# @shined/react-use 的现代化配置

---

```yaml
layout: two-cols
```

# 现代化配置

<v-clicks>
<div class='flex flex-col gap-12 mt-12'>
  <div class='flex items-center'>
    <span>仓库结构：</span>
    <vscode-icons-file-type-light-pnpm class="size-8 mx-2" /> pnpm + 
    <vscode-icons-file-type-git class="size-8 mx-2" /> Monorepo
  </div>
  <div class='flex items-center'>
    <span>格式化和规范检查：</span>
    <vscode-icons-file-type-biome class="size-8 mx-2" /> Biome +
    <img src="/oxlint.svg" class="inline size-8 mx-2" /> oxlint
  </div>
  <div class='flex items-center'>
    <span>测试与打包：</span>
    <vscode-icons-file-type-vitest class="size-8 mx-2" /> Vitest +
    <vscode-icons-file-type-esbuild class="size-8 mx-2" /> tsup (esbuild)
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
</div>
</v-clicks>

::right::

<v-clicks>
<div class='flex flex-col gap-12 mt-20'>
  <div class='flex flex-wrap items-center line-height-12'>
    <span>版本锁定：</span>
    <vscode-icons-file-type-node class="size-8 mx-2" /> .node-version + 
    <vscode-icons-folder-type-package class="size-8 mx-2" /> {{'<package.json>.packageManager'}}
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
  <!-- <div class='flex items-center'>
    <span>供应链安全：</span>
    <img src="/github-action.svg" class="inline size-8 mx-2" /> Action + 
    <mdi-verified class="text-[#327532] size-8 mx-2" /> npm Provenance
  </div> -->
</div>
</v-clicks>

---

```yaml
layout: center
```

# 一些思考

---

# State vs Ref

State 和 Ref 都可以用来保存信息，该如何抉择？

<v-click>

- 四种组件状态: 全局变量、局部变量、Ref、State
- 全局变量对于所有组件实例都是共享的 (状态混乱)
- 局部变量会在渲染阶段被重新声明 (<code>let</code> 和 <code>const</code>，状态丢失)
- 遵循组件生命周期且多次渲染仍稳定存在的状态，只有 State 和 Ref
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

</v-click>

---

# State

- 用于保留渲染之间数据的状态变量 (渲染稳定)。
- 状态对于组件的每个副本都是本地的 (实例隔离)。
- 更改它会触发重新渲染。

<br />

# Ref

- 可以在重新渲染之间存储信息 (渲染稳定)。
- 该信息对于组件的每个副本都是本地的 (实例隔离)。
- 更改它不会触发重新渲染 (不触发渲染，与 State 不同)。

---

# 介绍 Ref Getter

兼具信息存储功能与渲染优化，一种取舍与妥协

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

pause() // will not trigger re-render
pause(true) // will trigger re-render
```

<tip v-click><b>isActive</b> 状态的变更，默认不会触发重新渲染（可选），其值可通过 Getter 函数获取</tip>

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

<div v-click>还可以继续抽象</div>

<br />

<div v-click>

```tsx
const [isActiveRef, isActive] = useGetterRef(false)
```

</div>

<tip v-click>利用 <b>useGetterRef()</b> 来创建 Ref Getter，底层使用 Ref 而不是 State 以优化性能</tip>

---

```yaml
layout: center
```

# useSafeState 做了什么

---

# 问题背景

- 在一个 React 组件卸载后执行 <code>setState</code>，React (<=17) 会抛出警告
- 本质上，组件卸载后执行的 <code>setState</code> 只是一个 <code>noop</code> (空函数)
- 抛出的警告并不意味着内存泄漏，只是在提醒你清理定时器、订阅函数等可能导致内存泄漏的操作
- React 官方意识到了这个警告的不准确，从 React 18 开始移除了它
- 官方指出，组件卸载后应当执行 <code>setState</code>，未来可能会有「状态保持」等特性依赖于此
- 这个时候卸载后的 <code>setState</code> 将有意义，换句话说，卸载后不执行 <code>setState</code> 将会导致更严重的问题
- 因而在没有抛出警告时，不应当拦截 <code>setState</code> 操作（尽管目前而言只是 <code>noop</code>，没有实际意义）
- 更多讨论可以参考 [React Discussion #82](https://github.com/reactwg/react-18/discussions/82)

<img src="/set-state.png" class='mt-4 h-160px rounded' v-click />

---

## ahooks 的做法

- 组件卸载后，不执行 setState，以抑制抛出的警告
- 其余情况与 useState 一致

## @shined/react-use 的做法

- 具体行为依 React 版本不同而不同
  - React 17 及以下版本（会抛出警告），组件卸载后，不执行 setState，以抑制抛出的警告
  - React 18 及以上版本（警告被移除），不做任何处理，等同于 useState，遵循官方推荐做法
- 新增了可选的 deep 选项
  - 用于深度对比并决定是否更新，具备渲染优化，默认 false (不开启)

<br />

<v-click>

```tsx
const [value, setValue] = useSafeState({ count: 1 }, { deep: true })
setValue({ count: 1 }) // 更新「同样」的对象，如果未设置 deep，将触发组件重新更新
```

</v-click>

<tip v-click>在状态「简单、可控」时，一次 deep compare 的开销远比一次组件渲染小的多得多</tip>

---

```yaml
layout: center
```

# 单一职责

一次只做一件事，并把它尽可能做好

---

# 一套优秀的乐高积木有什么特点？

<div class='flex justify-between'>
  <img src='/lego.png' class='h-60 rounded' v-click />
  <div class='w-1/2 py-20 flex flex-col gap-8'>
    <div v-click>种类多而全</div>
    <div v-click>灵活可插拔</div>
  </div>
</div>

<tip v-click>种类多而全：零件种类多 (不管是功能还是颜色)、各司其职，得以实现各种各样的奇思妙想</tip>
<tip v-click>灵活可插拔：已经组装好的结构可再次拆卸或复用，或者能找到对应部件并能再次快速组装</tip>

---

```yaml
layout: center
```

# 逻辑组件 (Hooks) 应当像乐高积木一样

各司其职、灵活、可插拔

---

# ahooks 的缺陷

<v-clicks>

- 覆盖面较广 (70+)，但仍缺乏许多常见场景的 Hooks
  - 复制操作、剪切板读写
  - 浏览器取色器、FPS 等各种浏览器 API
  - 时间格式化等其他常见的高频简单逻辑
- 一个 Hook 里面包含了许多耦合的逻辑，导致难以进行拆分、复用和组合
  - 如: useRequest 内部包含了 loading 状态、轮询、错误重试、聚焦重加载、重连重加载 等逻辑
- 许多 Hooks 的职责和命名不够明确，边界模糊
  - 如: useInfiniteScroll 把所有状态都内聚了，对外部 store 等情况支持不友好
  - 如: usePagination 其实是 useRequest 的上层封装，而不是字面上的“分页”逻辑

</v-clicks>

<div class='mt-6 font-bold' v-click>
乐高视角：一套数量够用的零件，附带几辆「零件罕见、预装好但被粘牢」的大汽车，难以拆分和重组
</div>

<tip v-click>零件相对完善，大汽车在多数情况下也确实跑的很快，但丢失了乐高积木强调的灵活和趣味性</tip>

---

```yaml
layout: two-cols-header
```

# @shined/react-use 的做法

 一次只做一件事，且做到极致

::left::

<v-clicks>

- 逻辑组件 (Hooks) 的<span v-mark="{ at: 4, color: '#f59e0b', type: 'underline'}">单一职责</span>
  - 如: useClipboard 处理剪贴板读、写，支持常见相关操作与 API 自动降级
  - 如: usePagination 只处理分页状态，无额外逻辑
  - 如: useInfiniteScroll 只处理滚动加载逻辑，精确提供加载回调和滚动状态，状态处理交由用户
- 逻辑组件 (Hooks) 的<span v-mark="{ at: 5, color: '#f59e0b', type: 'underline'}">按需组合</span>
  - Hooks 细致拆分、按需组合，而不是一个 Hooks 内聚大量业务逻辑和状态
  - useRequest (基本成型) 组合了 13 个 Hooks 完成 80% 以上的核心逻辑，代码量不到 400 行

</v-clicks>

::right::

<img v-click src='/use-element-by-pointer.png' class='mx-4 h-80 rounded' />

---

```yaml
layout: center
```

# useRequest 能力预览

当前处于开发后期阶段，核心功能基本完善

---

# useRequest 能力预览

作为后期大多数业务场景 Hooks 的基础逻辑，useRequest 是重中之重

- 基础场景：管理异步请求，自动处理 loading、error、data 等状态（useAsyncFn）
- 依赖收集：用到什么状态，跟踪什么状态 （按需渲染，底层 Getter 依赖标记）
- 请求轮询：定期自动发起请求更新数据，时刻保持数据最新（useIntervalFn）
- 错误重试：支持错误重试，自动处理错误状态，重试请求（useRetryFn）
- 生命周期：完整的请求生命周期支持，包括 onBefore、onSuccess、onError、onFinally 等
- 聚焦重加载：在页面重新聚焦时，自动重新加载数据 （useReFocus，支持注册自定义处理函数）
- 重连重加载：在断网重连后，自动重新加载数据 （useReConnect，支持注册自定义处理函数）
- 依赖刷新：在依赖变化时，自动重新请求数据 （useUpdateEffect）
- 慢加载状态：自动处理慢加载状态，提供慢加载回调 （useLoadingSlowFn）
- 防抖和节流：手动执行时，支持通过防抖和节流进行频率限制 （useDebouncedFn、useThrottledFn）
- 数据缓存：支持 `provider` 选项，通过一个「类 Map」的数据结构进行缓存操作（Reactive/LocalStorage）
- 多环境支持：支持 SSR、React Native、Ink、小程序 等诸多场景，不强绑定 Web

---

```yaml
layout: center
```

# 关于 React 19

---

# React 19 主要更新速览

<v-clicks>

- 正式引入「Actions」概念
  - startTransition 支持异步 Action，自动处理状态、错误、乐观更新等
- 新 Hooks
  - useOptimistic, useActionState (前 useFormState), useFormStatus
- 新 API
  - use 用于消费渲染过程中的资源 (如: 非渲染过程中创建的 Promise)
- 新的范式
  - React Server Components + Server Actions
- 其他改进:
  - 废弃 forwardRef, ref 可直接作为 props 传递, Element 的 ref 参数支持返回清理函数
  - `<Context />` 可以替代 `<Context.Provider />` 直接进行渲染
  - 更多 Document Metadata 的支持，直接编写 `<title />`, `<link />`, `<meta />` 等元素
  - 全面支持 HTML 原生自定义组件 (Custom Elements)，废弃组件的 defaultProps 属性

</v-clicks>

---

# React 19 现状

<v-clicks>

- 两个事实
  - React 19 到目前为止，仍未发布正式版本 (现在仍是 RC 版本)
  - React 19 最重要的改动是新增的几个 Hooks，实际业务价值有限
    - 针对 form 和异步网络请求通用能力的封装，与 form 耦合严重
    - 对于实际业务使用成本相对较高 (自定义操作需要用 startTransition 包裹)
- 一个现实
  - React 19 包含 Breaking Change 的新特性和变更
    - 这些变更在短期内对公司项目来说改造和升级不太容易
    - 如：React 19 废弃了 defaultProps，shineout@3 中仍有使用

</v-clicks>

<tip v-click>针对 React 19 的 Hooks 开发有必要，但是相对而言优先级不高，会放到后续阶段进行跟进</tip>

---

```yaml
layout: center
```

# Hooks 使用引导 / 高频预测

---

# Hooks 使用引导 / 高频预测

<v-clicks>

- **生命周期**：useMount / useUnmount / useUpdateEffect / useEffectOnce
- **异步操作**：useRequest / useAsyncFn (useLoadingFn)
- **状态相关**：useSafeState / useControlledComponent / useBoolean / useCircularList
- **浏览器相关**：useMouse / useScroll / useElementSize / useFullscreen / useNetwork
- **节流/防抖/定时器**：useDebouncedFn / useThrottledFn / useIntervalFn
- **强业务/场景化**：useInfiniteScroll / useDynamicList / useCountdown / useMultiSelect
- **时间格式化**：useDateFormat (dayjs、moment、date-fns 简单使用场景的平替)
- **复制操作 (剪贴板)**：useClipboard (读、写，自动降级到 <code>document.exec('copy')</code>)
- **Hooks 老手/上层封装**: usePausable / useSupported / useStableFn / useLatest / useCreation

</v-clicks>

---

```yaml
layout: center
```

# @shined/react-use 的后续规划

---

# 后续规划

<v-clicks>

- 起步期（已基本结束）
  - 以原子化为主，也包含部分场景化 Hooks
  - 覆盖大部分场景，已生产可用 （目前 140+ 个）
- 业务强化期（当前阶段）
  - 封装「强业务」的上层 Hooks
  - 做到在常见场景下，「几行代码实现开箱即用」
- 生态完善期
  - 与 shineout 等主流库做集成联动，进行上层封装
  - 简化业务开发的同时，扩大 Hooks 周边生态

</v-clicks>

---

```yaml
layout: center
```

# 回顾

---

# 回顾

<v-clicks>

- Hooks 与 useCounter
  - React 的重要事件节点，Hooks 开发成为主流
  - Hooks 可以被灵活组合，提出「逻辑组件」(Hooks) 概念
  - 改进 useCounter，了解 Hooks 的常见陷阱与最佳实践
- <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-6 inline" /> <code>@shined/react-use</code>
  - 全面、轻量、灵活、高度优化、多环境友好、出色的开发体验、现代化配置
- 一些思考
  - State 和 Ref、Ref Getter，兼具信息存储与渲染优化
  - useSafeState 的设计与思考
  - 单一职责，逻辑组件应当可按需组合
  - useRequest 能力预览
  - React 19 新特性和当前业务价值
- Hooks 的使用预测及后续规划

</v-clicks>

---

```yaml
layout: end
```

# The End

感谢大家的耐心收听

## （Q & A）
