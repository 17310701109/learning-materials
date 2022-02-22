# Android

## Activity

**在一个android应用中，一个Activity通常就是一个单独的屏幕，它上面可以显示一些控件也可以监听并处理用户的事件做出响应。Activity之间通过Intent进行通信。**

**参考链接：**[Activity的36个难点](https://juejin.cn/post/6844903983656468488#heading-4)

## 生命周期

**生命周期图**

![](C:\Users\ding\Desktop\资料\Image\ActivityLifecycle.webp)



### 横竖屏切换时的生命周期

- 不设置 `Activity` 的 `android:configChanges` 时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次

- 设置 `Activity` 的 `android:configChanges="orientation"` 时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次

- 设置 `Activity` 的 `android:configChanges="orientation|keyboardHidden"` 时，切屏不会重新调用各个生命周期，只会执行 `onConfigurationChanged` 方法

- 注意：还有一点，非常重要，一个 `Android` 的变更细节！当 `API >12` 时，需要加入 `screenSize` 属性，否则屏幕切换时即使你设置了 `orientation` 系统也会重建 `Activity` ！

### 谁管理的Activity生命周期？





## 启动模式

- `Standard` 模式（默认模式）

  每次启动一个 `Activity` 都会又一次创建一个新的实例入栈，无论这个实例是否存在。

- `SingleTop` 模式（栈顶复用模式）

  分两种处理情况：须要创建的 `Activity` 已经处于栈顶时，此时会直接复用栈顶的 `Activity` 。不会再创建新的 `Activity` ；若须要创建的 `Activity` 不处于栈顶，此时会又一次创建一个新的 `Activity` 入栈，同 `Standard` 模式一样。

- `SingleTask` 模式（栈内复用模式）

  若须要创建的 `Activity` 已经处于栈中时，此时不会创建新的 `Activity` ，而是将存在栈中的 `Activity` 上面的其他 `Activity` 所有销毁，使它成为栈顶。

- `SingleInstance` 模式（单实例模式）

  `SingleInstance` 比较特殊，是全局单例模式，是一种加强的 `SingleTask` 模式。它除了具有它所有特性外，还加强了一点：只有一个实例，并且这个实例独立运行在一个 `task` 中，这个 `task` 只有这个实例，不允许有别的 `Activity` 存在。

**启动模式实际应用场景**

- `SingleTask` 模式的运用场景

1. 最常见的应用场景就是保持我们应用开启后仅仅有一个 `Activity` 的实例。
2. 最典型的样例就是应用中展示的主页（ `Home` 页）。
3. 假设用户在主页跳转到其他页面，运行多次操作后想返回到主页，假设不使用 `SingleTask` 模式，在点击返回的过程中会多次看到主页，这明显就是设计不合理了

- `SingleTop` 模式的运用场景

1. 假设你在当前的 `Activity` 中又要启动同类型的 `Activity`
2. 此时建议将此类型 `Activity` 的启动模式指定为 `SingleTop` ，能够降低Activity的创建，节省内存！

- 注意：复用 `Activity` 时的生命周期回调

1. 这里还须要考虑一个 `Activity` 跳转时携带页面參数的问题。
2. 由于当一个 `Activity` 设置了 `SingleTop` 或者 `SingleTask` 模式后，跳转此 `Activity` 出现复用原有 `Activity` 的情况时，此 `Activity` 的 `onCreate` 方法将不会再次运行。`onCreate` 方法仅仅会在第一次创建 `Activity` 时被运行。
3. 而一般 `onCreate` 方法中会进行该页面的数据初始化、`UI` 初始化，假设页面的展示数据无关页面跳转传递的參数，则不必操心此问题
4. 若页面展示的数据就是通过 `getInten()` 方法来获取，那么问题就会出现：`getInten()` 获取的一直都是老数据，根本无法接收跳转时传送的新数据！