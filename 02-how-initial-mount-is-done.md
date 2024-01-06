# How does React do the initial mount internally?

- 원본
  - [2. How does React do the initial mount internally?](https://jser.dev/2023-07-14-initial-mount/)

<br />

---

<br />

## 1. Brief introduction on Fiber Architecture

<img src="https://github.com/muilyang12/react-internals-deep-dive/assets/78548830/b0d0f434-156e-4b03-8c1f-1edc635fbf18" width=750 />

<br />
<br />

- Fiber는 React가 앱 상태의 내부 표현을 어떻게 유지하는지를 표현한 구조입니다. 이 구조는 FiberRootNode와 FiberNodes로 구성된 트리 형태의 구조를 갖습니다. 여러 종류의 FiberNode가 존재하는데, 그 중 일부는 HostComponent로 DOM 노드를 가지고 있는 노드입니다.

- React 런타임은 Fiber 트리를 유지하고 업데이트하며, 최소한의 조작으로 DOM의 업데이트를 동기화합니다.

<br />

#### FiberRootNode

- FiberRootNode는 React의 루트로 작용하는 특별한 노드로, 앱에 대한 메타 정보를 보유한 노드입니다. FiberRootNode의 'current' 값은 실제 Fiber 트리를 가리킵니다. 그리고 새로운 Fiber 트리가 구성될 때마다, 그것의 'current' 값은 새로운 HostRoot를 가리키게 됩니다.

<br />

#### FiberNode

- FiberNode는 FiberRootNode 외의 다른 노드들을 의미하며, 몇 가지 중요한 속성들은 가집니다.
  1. tag: FiberNode는 tag에 의해 구별되는 여러 하위 유형을 가지고 있습니다. 예를 들어, FunctionComponent, HostRoot, ContextConsumer, MemoComponent, SuspenseComponent 등이 있습니다.
  2. stateNode: 뒤에 있는 다른 데이터를 가리키는 데 사용하는 속성입니다. 예를 들어 HostComponent의 stateNode는 실제 DOM 노드를 가리킵니다.
  3. child, sibling, return: 나무와 같은 구조를 형성하는 데에 필요한 속성입니다.
  4. elementType: 제공된 컴포넌트 함수나 내부 HTML 태그를 표현하는 속성입니다.
  5. flags: Commit 단계에서 수행해야 할 작업이 있음을 나타내기 위한 속성입니다. 예를 들어 subtreeFlags는 해당 노드의 하위 트리에 대한 flags 속성입니다.
  6. lanes: 보류 중인 업데이트의 우선 순위를 나타내는 데에 필요한 속성입니다. childLanes는 해당 노드의 하위 트리에 대한 lanes 속성입니다.
  7. memoizedState: 해당 노드에게 중요한 데이터 가리키는 데에 사용하는 속성입니다. FunctionComponent의 경우 그것의 hooks를 가리키게 됩니다.
