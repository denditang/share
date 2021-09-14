### HOC

hoc本质上是一个函数，该函数接受一个组件作为输入，还可以传递其他的参数，基于该组件返回一个不同的组件
```
function withSubscription(WrappedComponent, selectData) {
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        data: selectData(DataSource, props)
      };
    }
    // 一些通用的逻辑处理
    render() {
      // ... 并使用新数据渲染被包装的组件!
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```
优点：
* 逻辑复用，不影响被包裹组件的内部逻辑

缺点：
* 固定的props可能会被覆盖
* 它无法清晰的标识数据的来源，它不糊告诉你组件包含了那些props，增加了调试和修复代码的时间

### render props

render prop 是指一种在react组件之间使用一个值为函数的prop共享代码的简单技术。
具有render  prop的组件接受一个返回react元素的函数，将render的渲染逻辑注入到组件内部。
在这里 render的命名可以是任何其他有效的标识符。
```
class DataProvider extends React.Components {

    state = {
        name:'TOM'
    }
    render (){
        return (
            <div>
                <p>共享数据组件自己的内部的渲染逻辑 </p>
                {this.props.render(this.state)}
            </div>
        )
    }
}
<DataProvider render={data =>(
    <h1>hello {data.name}</h1>
)}
```
优点：
* 数据共享 代码复用，将组件内的state作为props传递给调用者，将渲染逻辑交给调用者

缺点：
* 无法在return语句外访问数据，嵌套写法不够优雅

### hooks

hooks 是一些可以让你在函数组件里钩入react state及生命周期等特性的函数。hook不能在class组件中使用


hook就是js函数，但是使用它们会有两个额外的规则
* 只能在函数最外层使用hook ,不要在循环 条件判断 或者子函数中调用
* 只能在react的函数组件中调用hook ，不要在其他js函数中调用。


### 三者实现react双向绑定的功能


#### hoc

``` 
const HocBind = WrapperComponent =>{
  return class extends React.Component{

    state={
      value:this.props.initialValue
    };
    onChange = e =>{
      this.setState({value:e.target.value})
      if(this.props.onChange){
        this.props.onChange(e.target.value)
      }
    }
    render(){
      const newProps = {
        value:this.state.value,
        onChange:this.onChange
      }
      return <WrapperComponent {...newProps} />
    }
  }
}
const TxInput = props =>(
  <>
     <p>value:{props.value}</p>
     <input {...props} />
  </>
)
const HocInput = HocBind(TxInput)

      <HocInput
        initialValue="init"
        onChange={val => {
          console.log("HocInput", val);
        }}
      />

```

#### render props


```
class HocPropsBind extends React.Component{
  constructor(props){
    super(props)
    this.state = {
      value:props.initialValue
    }
  }
  onChange = e =>{
    this.setState({
      value:e.target.value
    })
    if(this.props.onChange){
      this.props.onChange(e.target.value)
    }
  }
  render(){
    return (
      <>
      {this.props.children({
        value:this.state.value,
        onChange:this.onChange
      })}
      </>
    )
  }
}

<HocPropsBind
initialValue="init"
onChange={val =>{
  console.log('hocbind',val)
}}
>
  {props =>(
    <>
    <p> hoc bind实现 value:{props.value} </p>
    <input {...props} />
    </>
  )}
</HocPropsBind>
```


#### hooks写法

```
function useBind(initialValue){
  const [value,setValue] = useState(initialValue)
  const onChange = e =>{
    setValue(e.target.value)
  }
  return [value,onChange]
}

function InputBind() {
  const inputProps = useBind("init");
  return (
    <p>
      <p>useBind实现 value:{inputProps.value}</p>
      <input {...inputProps} />
    </p>
  );
}

```