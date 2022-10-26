# Context

- Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法
- 可以跨层级传输数据

```tsx
const MyContext: React.Context<string> = React.createContext('defaultValue');
const MyContext2: React.Context<string> = React.createContext('defaultValue2');

function PageChild2() {

 return <MyContext.Consumer>
  {(c1: string) => (
   <MyContext2.Consumer>
    {(c2: string) => (
     <div>{`c1:${c1}; c2:${c2}`}</div>
    )}
   </MyContext2.Consumer>
  )}
 </MyContext.Consumer>
}


class PageChild extends React.Component {
 render() {
  return <div>child {JSON.stringify(this.context)}</div>
 }
}

PageChild.contextType = MyContext;


function Page(props: any) {
 return (
  <MyContext.Provider value={'newValue'}>
   <MyContext2.Provider value={'newValue2'}>
    <PageChild />
    <PageChild2 />
   </MyContext2.Provider>
  </MyContext.Provider>
 );
}
```
