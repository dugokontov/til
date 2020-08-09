## `forwardRef`

Ref forwarding is a technique for automatically passing a [`ref`](https://reactjs.org/docs/refs-and-the-dom.html) through a component to one of its children. [[react documentation](https://reactjs.org/docs/forwarding-refs.html)]

`forwardRef` function accepts a function with two arguments (`props` passed to component and `ref`) and returns a React node.

### In typescript

In typescript `forwardRef` function is defined as a generic function that accepts two generic types [[index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/7914a6af72ed1f95ee850b1f82e1beee189fbb97/types/react/index.d.ts#L795)]:

 1. Type of component `ref` will be attached to
 1. Props that will be passed to component created using `forwardRef` function

This is in reverse compared to the order of arguments passed to method that `forwardRef` uses.

The correct way to use `React.forwardRef` is as follows:

```ts
export type Props = {
    // prop types need for this component
} & React.ButtonHTMLAttributes<HTMLButtonElement>;

const Button = React.forwardRef<HTMLButtonElement, Props>(
    (props, ref) => <button ref={ref} {...props} />
);
```

Thanks to Kristofer Giltvedt Selbekk for his [blog post](https://www.selbekk.io/blog/2020/05/forwarding-refs-in-typescript/). This was the only place where I could fine nice example of how to properly use `forwardRef` in typescript.