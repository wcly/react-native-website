---
id: pressable
title: Pressable
---
Pressable是一个核心组件包装器，它可以检测定义在它内部的任何子组件上的点击交互的不同阶段。

```jsx
<Pressable onPress={onPressFunction}>
  <Text>I'm pressable!</Text>
</Pressable>
```

## 它是怎么工作的

当一个元素被`Pressable`包裹:

1. [`onPressIn`](#onpressin) 在按下时，触发`onPress`之前调用.
2. [`onPress`](#onpress) 在单按手势触发时，`onPressIn`触发的130毫秒后调用.
3. [`onLongPress`](#onlongpress) 只有在按下时，`onPressIn` 触发后超过500毫秒（或设置的[`delayLongPress`](#delaylongpress)属性）才会调用。
4. [`onPressOut`](#onpressout) 当按下手势停用时被调用.

  <img src="https://cdn.jsdelivr.net/gh/reactnativecn/react-native-website@gh-pages/docs/assets/d_pressable_pressing.svg" width="1000" alt="Diagram of the onPress events in sequence.">

Fingers are not the most precise instruments, and it's not uncommon for users to "fat finger" an interface—to activate the wrong thing or miss the activation area. To help, `Pressable` has an optional `HitRect` you can use to define how far a touch can register away from the the wrapped element. Presses can start anywhere within a `HitRect`.

`PressRect` allows presses to move beyond the element and its `HitRect` while maintaining activation and being eligible for a "press"—think of sliding your finger slowly away from a button you're pressing down on.

> The touch area never extends past the parent view bounds and the Z-index of sibling views always takes precedence if a touch hits two overlapping views.

<figure>
  <img src="https://cdn.jsdelivr.net/gh/reactnativecn/react-native-website@gh-pages/docs/assets/d_pressable_anatomy.svg" width="1000" alt="Diagram of HitRect and PressRect and how they work.">
  <figcaption>
    You can set <code>HitRect</code> with <code>hitSlop</code> and set <code>PressRect</code> with <code>pressRetentionOffset</code>.
  </figcaption>
</figure>

> `Pressable` uses React Native's `Pressability` API. For more information around the state machine flow of Pressability and how it works, check out the implementation for [Pressability](https://github.com/facebook/react-native/blob/16ea9ba8133a5340ed6751ec7d49bf03a0d4c5ea/Libraries/Pressability/Pressability.js#L347).

## Example

```js
import React, { useState } from 'react';
import { Pressable, StyleSheet, Text, View } from 'react-native';

export default App = () => {
  const [timesPressed, setTimesPressed] = useState(0);

  let textLog = '';
  if (timesPressed > 1) {
    textLog = timesPressed + 'x onPress';
  } else if (timesPressed > 0) {
    textLog = 'onPress';
  }

  return (
    <View>
      <Pressable
        onPress={() => {
          setTimesPressed((current) => current + 1);
        }}
        style={({ pressed }) => [
          {
            backgroundColor: pressed
              ? 'rgb(210, 230, 255)'
              : 'white'
          },
          styles.wrapperCustom
        ]}>
        {({ pressed }) => (
          <Text style={styles.text}>
            {pressed ? 'Pressed!' : 'Press Me'}
          </Text>
        )}
      </Pressable>
      <View style={styles.logBox}>
        <Text testID="pressable_press_console">{textLog}</Text>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  text: {
    fontSize: 16
  },
  wrapperCustom: {
    borderRadius: 8,
    padding: 6
  },
  logBox: {
    padding: 20,
    margin: 10,
    borderWidth: StyleSheet.hairlineWidth,
    borderColor: '#f0f0f0',
    backgroundColor: '#f9f9f9'
  }
});
```

## Props

### `android_disableSound`

如果为真，点击时不播放Android系统声音。

| Type    | Required | Platform |
| ------- | -------- | -------- |
| boolean | No       | Android  |

### `android_rippleColor`

启用Android涟漪效果并配置其颜色。

| Type | Required | Platform |
| --- | --- | --- |
| [color](https://reactnative.dev/docs/colors) | No | Android |

### `children`

Either children or a function that receives a boolean reflecting whether the component is currently pressed.

| Type       | Required |
| ---------- | -------- |
| React.Node | No       |

### `delayLongPress`

Duration (in milliseconds) from `onPress` before `onLongPress` is called.

| Type   | Required | Default |
| ------ | -------- | ------- |
| number | No       | 500     |

### `disabled`

Whether the press behavior is disabled.

| Type    | Required |
| ------- | -------- |
| boolean | No       |

### `hitSlop`

Sets additional distance outside of element in which a press can be detected.

| Type       | Required |
| ---------- | -------- |
| RectOrSize | No       |

### `onLongPress`

Called when a press event lasts longer than 500 milliseconds. Delay can be customized with [`delayLongPress`](#delaylongpress).

| Type       | Required |
| ---------- | -------- |
| PressEvent | No       |

### `onPress`

Called when a single tap gesture is detected, 130 milliseconds after `onPressIn`.

| Type       | Required |
| ---------- | -------- |
| PressEvent | No       |

### `onPressIn`

Called immediately when a touch is engaged, before `onPress`.

| Type       | Required |
| ---------- | -------- |
| PressEvent | No       |

### `onPressOut`

Called when a touch is released.

| Type       | Required |
| ---------- | -------- |
| PressEvent | No       |

### `pressRetentionOffset`

Additional distance outside of this view in which a touch is considered a press before `onPressOut` is triggered.

| Type         | Required |
| ------------ | -------- |
| Rect or Size | No       |

### `style`

Either view styles or a function that receives a boolean reflecting whether the component is currently pressed and returns view styles.

| Type | Required |
| --- | --- |
| [ViewStyleProp](https://reactnative.dev/docs/view-style-props) | No |

### `testOnly_pressed`

Used only for documentation or testing (e.g. snapshot testing).

| Type    | Required |
| ------- | -------- |
| boolean | No       |
