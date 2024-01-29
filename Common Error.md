# Jest总结
## 常见报错
### 1.usevisibilityContest must be used within a Visibility Provider.
    Provider没有传context,测试文件渲染组件的位置写错了，要参考业务代码的组件渲染方式，通常与之相同。
### 2，act函数报错
    如果是模拟事件行为，用await waitFor(()=>...)
### 3,react.jsx:type is invalid,expected a string/class component/function component,but got undefined.
    如果组件import/export没写错，尝试渲染组件时外面包裹一层（新组件返回该组件）
    test('test div component',()=>{
        const Com = ()=>{
            return (<div>123</div>);
        };
         render(
            <PageContainer>
                <Com />
            </PageContainer>
        );
    })
### 4，can not destructure xxx of xxx.
    业务代码中不要解构该对象属性
### 5，found multiple elements by xxx.
    同个组件复用了多次，使用props传递data-testid
### 6，setState报错
    setState控制的业务代码内部出错；监控表单数据来setState
### 8，异步请求的数据，没覆盖上
    测试文件给数据初始值不要给空数据
### 7，branch覆盖率难以提升
    不要用三元运算符，少用&& ??运算符
### 8，函数覆盖率为0
    组件没测试到，函数没调用
### 9，A component is changing the uncontrolled value state of RadioGroup to be controlled.
    受控组件没有给初始值，需要给初始值。formik中给了initailValues但还是error，还没解决。
### 10, network forbidden报错，原因是页面的api没有mock到，以下方法mock api写法简单
    import * as request from '~/utils/request;
    
    jest.mock(~/utils/request);//api所在的文件，如果api是写在hook里面，就要mock那个hook所在的文件
    const mockApi = jest.spyon(request,'要mock的api')；
    mockApi.mockImplementation(():any=>
        Promise.resolve('api返回的数据');
    );
### 11, 某些组件没有覆盖到，检查是不是数据给得不对，比如某个组件需要props数据为指定数据才会渲染，就要在test文件中mock需要的props数据
   ```
    //test文件
    const data = {name:'rose'};//要给指定的数据
    <Ele data={data} />；
    //页面文件
    {props.data.name==='rose' && <Ele />} 
   ```
### 12, 对于mui-material的Dialog组件，找元素不能用data-testid，这样会报Unable to find an element，要用getByAllText('submit')[0]//submit和[0]根据实际情况而定
### 13, mock页面中的api：
```
import * as api from './query';
import getData from './api;

//mock用useQuery包了一层的api
const mockApi = jest.spyOn(api,'oneApi');
mockApi.mockImplementation(
    ()=>mockApiData as any
);

//直接mock api
(getData as jest.Mock).mockImplementation(()=>mockgetDataApiRes);
```

    
    
    



