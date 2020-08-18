## Define redux store's properties
While trying to solve this I learned two things:

 - [Module augmentation](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation) enables typescript to augment existing modules (even 3rd party modules) with custom definition.
 - [Merging interfaces](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#merging-interfaces) in nutshell enables us to add new properties to existing interface definition.

### Why?
While working in complex redux applications, defining all the properties that are part of redux store is quite handy. It helps make less errors in selectors and reducers. Initially I did it by defining my own interface called `Store`. Every selector accepted first parameter of this interface. However, react-redux's hook `useSelector` by default works with `DefaultRootState`. To see what this interface does, I opened [definition for it](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/182b6ee8753ea741109c00e0437a0236bdf150f2/types/react-redux/index.d.ts#L49). There I found link to module augmentation in typescript.

### How?
Open up module definition using `declare module` keyword. Inside add new interface definition of the existing `DefaultRootState` interface. Typescript will merge those two definition into one.

```ts
// in types.ts

declare module 'react-redux' {
    interface DefaultRootState {
        status: Status;
        reportView: ReportView;
        tablesByGuid: TablesByGuid;
    }
}

```
