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
