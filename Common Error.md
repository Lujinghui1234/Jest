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
    
    
    
    



