# Calling actions outside a React event handler in pre React 18

Because React handles `setState` synchronously if it's called outside an event handler. Updating the state outside an event handler will force react to update the components synchronously, therefore adding the risk of encountering the zombie-child effect.
In order to fix this, the action needs to be wrapped in `unstable_batchedUpdates`

```jsx
import { unstable_batchedUpdates } from 'react-dom' // or 'react-native'

const useStore = create((set) => ({
  fishes: 0,
  increaseFishes: () => set((prev) => ({ fishes: prev.fishes + 1 })),
}))

const nonReactCallback = () => {
  unstable_batchedUpdates(() => {
    useStore.getState().increaseFishes()
  })
}
```

More details: https://github.com/pmndrs/zustand/issues/302
