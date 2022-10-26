# React.children.map()

> 获取子组件进行拷贝子组件添加新属性以便生成新组件的操

```tsx
class Child extends Component {
    componentDidMount() {
        console.log(
            React.Children.map(this.props.children, item => {
                return [item, [item]];
            })
        );
    }

    render() {
        return (
            <div>
                {React.Children.map(this.props.children, item => [
                    item,
                    [item]
                ])}
            </div>
        );
    }
}

class ChildrenMap extends Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <Child>
                <div>1</div>
                <div>2</div>
            </Child>
        );
    }
}

export default ChildrenMap
```
