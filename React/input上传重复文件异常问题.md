![photo-1619319.jpeg](https://i.loli.net/2021/06/08/YrqlKkiFItfz2p4.jpg)

# `input[type='file']` 连续上传同一个文件不触发 `onChange` 事件 或 Upload 组件只调用了一次 `onChange` 函数

原因为 `onChange` 的触发条件是 `value`的变化。当我们选取文件上传后，`value`的值为该文件在磁盘中的地址。如：`D:\test\1.docx` 。因此，我们改变`value`值即可。

## 背景一：原生`input`

如果使用的是原生`input`标签，只需在点击事件的时候置空`value`值即可。

```tsx
<input
  type="file"
  accept=".docx"
  onClick={(e) => {
    (e.target as HTMLInputElement).value = "";
  }}
  onChange={(e) => {
    console.log(`e`, e.target.files);
  }}
/>
```

## 背景二：采用了`Antd`的`Input`组件上传`file`

此时不能直接的使用背景一的方法去改变`value`，否则你会得到以下信息：

```shell
Uncaught DOMException: Failed to set the 'value' property on 'HTMLInputElement': This input element accepts a filename, which may only be programmatically set to the empty string.
```

> 翻译为：无法在“`HTMLInputElement`”上设置“`value`”属性：此输入元素接受文件名，该文件名只能以编程方式设置为空字符串。

这对于文件输入无效，因为浏览器只允许读取而不是写入输入。

可调用`Input`身上的`setValue(value: string, callback?: () => void): void`方法，即可解决。

```tsx
const uploadIptRef = useRef<Input>(null);
<Input
  ref={uploadIptRef}
  type="file"
  accept=".docx"
  onClick={(e) => {
    uploadIptRef.current?.setValue("");
  }}
  onChange={(e) => {
    console.log(`e`, e.target.files);
  }}
/>;
```

### `setValue`在`ant-design\components\input\Input.tsx`中表现为

```ts
setValue(value: string, callback?: () => void) {
    if (this.props.value === undefined) {
        this.setState({ value }, callback);
    } else {
        callback?.();
    }
}
```

渲染组件

```tsx
renderComponent = ({ getPrefixCls, direction, input }: ConfigConsumerProps) => {
  const { value, focused } = this.state;
  const { prefixCls: customizePrefixCls, bordered = true } = this.props;
  const prefixCls = getPrefixCls("input", customizePrefixCls);
  this.direction = direction;

  return (
    <SizeContext.Consumer>
      {(size) => (
        <ClearableLabeledInput
          size={size}
          {...this.props}
          prefixCls={prefixCls}
          inputType="input"
          value={fixControlledValue(value)}
          element={this.renderInput(prefixCls, size, bordered, input)}
          handleReset={this.handleReset}
          ref={this.saveClearableInput}
          direction={direction}
          focused={focused}
          triggerFocus={this.focus}
          bordered={bordered}
        />
      )}
    </SizeContext.Consumer>
  );
};
```

## 背景三：采用了`Ant design`的`Upload`组件，遇到`onChange`只调用一次问题

可参考 [#2423](https://github.com/ant-design/ant-design/issues/2423)
