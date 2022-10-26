# Mobx



## this找不到问题

```tsx
 添加	
 constructor() {
		makeObservable(this);
	}
```



## 继承使用

### 父类

```tsx
class father{
   constructor() {
		makeObservable(this);
	}
  @action.bound
  fn(){}
}

```



### 子类

```tsx
class son extends father{
   constructor() {
     super();
		makeObservable(this);
	}
  @override
	fn(){
		// 这里就可以重写方法
  }  
}
```

