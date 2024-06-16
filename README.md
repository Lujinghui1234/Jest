# Jest总结
## 常见报错
### 1. usevisibilityContest must be used within a Visibility Provider.
    Provider没有传context,测试文件渲染组件的位置写错了，要参考业务代码的组件渲染方式，通常与之相同。
### 2. act函数报错
    如果是模拟事件行为，用await waitFor(()=>...)
### 3. react.jsx:type is invalid,expected a string/class component/function component,but got undefined.
    检查mock的组件哪里写错了,jest认为这个组件是undefined
### 4. can not destructure xxx of xxx.
    业务代码中不要解构该对象属性
### 5. found multiple elements by xxx.
    同个组件复用了多次，可以通过使用props传递data-testid来解决，这样每个组件都会是独立的
### 6. setState报错
    setState控制的业务代码内部出错；可以通过监控表单数据来setState
### 8. 异步请求的数据，没覆盖上
    测试文件给数据初始值不要给空数据
### 7. branch覆盖率难以提升
    不要用三元运算符，少用&& ??运算符
### 8. 函数覆盖率为0
    组件没测试到，函数没调用
### 9. A component is changing the uncontrolled value state of RadioGroup to be controlled.
    受控组件没有给初始值，需要给初始值。formik中给了initailValues但还是error，还没解决。
### 10. network forbidden报错，原因是页面的api没有mock到，以下方法mock api写法简单
    import * as request from '~/utils/request;
    
    jest.mock(~/utils/request);//api所在的文件，如果api是写在hook里面，就要mock那个hook所在的文件
    const mockApi = jest.spyon(request,'要mock的api')；
    mockApi.mockImplementation(():any=>
        Promise.resolve('api返回的数据');
    );
### 11. 某些组件没有覆盖到，检查是不是数据给得不对，比如某个组件需要props为指定数据才会渲染，就要在test文件中mock需要的props数据
   ```
    //test文件
    const data = {name:'rose'};//要给指定的数据
    <Ele data={data} />；
    //页面文件
    {props.data.name==='rose' && <Ele />} 
   ```
### 12. 对于mui-material的Dialog组件，查找元素不能用data-testid，这样会报Unable to find an element，要用getByAllText('submit')[0]//submit和[0]根据实际情况而定
### 13. mock页面中的api：
1,第一种写法
```
import * as query from './query';
import getData from './api;
import {mockQueryData,mockApiData} from './mockApiData';

//mock用useQuery包了一层的api
const mockApi = jest.spyOn(query,'oneQueryFunc');
mockApi.mockImplementation(
    ()=>mockQueryData as any
);

//直接mock api
(getData as jest.Mock).mockImplementation(()=>mockApiData);
```

2,第二种写法：
```
jest.mock('~/api-v2/enquiry/api',()=>({
getData:jest.fn().mockResolvedValue(mockData)
))
```
### 14. 报错：getData(...).then is not a function //这里的getData是个api函数
```
//api
await getData()
.then((res:any)=>{
...
})
.catch((err:any)=>{
...
});

//jest  要这样写才可以
(getData as jest.Mock).mockResolvedValue(mockApiData);
```
### 15. 某个mock API或异步事件没有触发，要写在await waitFor里面
```
await waitFor(()=>expect(mockApi).toBecalled());
```
### 16. expect.any(Function)  用于验证某个值是否是一个函数的实例
```
//business code:
<DdeBottomAppBar onOpen={()=>handel(true)} />
```
```
//jest code:
const mockDdeBottomAppBar = jest.fn();
expect(mockDdeBottomAppBar).toBeCalledWith({
   onOpen:expect.any(Function)
})
```
### 17. mockReturnValue和mockResolvedValue有什么区别？
jest.mocked(api).mockReturnValue(mockData);
......待补充
### 18. 对于某些没有逻辑只有纯render的组件，jest中如何写expect?
```
const mockCom = jest.fn();
expect(mockCom).toHaveBeenCalledTimes(1);
```

