
# React API

```ts

type RedirectFun = (url:string) => void;
//新建表单记录
class New extends React.Component<{redirect:RedirectFun}, {}> {

}

//修改表单记录
class Edit extends React.Component<{redirect:RedirectFun}, {}> {

}

enum FieldType {
    SingleLine,
    MultiLine,
    Number,
    Decimal,
    Phone,
    Email,
    Date,
    Time,
    DateTime,
    Dropdown,
    Radio,
    Checkbox,
    FileUpload,
    ImageUpload,
}


type CreateFieldElementFun = (...args) => React.ReactElement;
interface EasyCode {
    //注册field的渲染组件
    void registerFieldComponent(field_type: FieldType, c: CreateFieldElementFun);
}

```




# TODO
* support vue