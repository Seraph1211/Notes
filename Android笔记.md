# Activity

Activity是Android四大组件之一，负责与用户的交互，通过setContentView()方法来显示指定控件/布局。

Activity之间可以通过Intent进行通信。

## Activity生命周期中的四大状态：

1. **运行状态**：当Activity处于返回栈的栈顶时（处于屏幕最前端，是可见的、有焦点的、可交互的），就被称为处于运行状态。当内存不足时，系统会优先销毁处于栈底的Activity，以确保栈顶Activity的正常运行（从用户体验的角度考虑）。
2. **暂停状态**： 当Activity不再处于栈顶，但是依然可见时（比如弹出一个dialog，或者一些没有占满屏幕的Activity），就进入了暂停状态。只有内存极低的情况下，系统才会考虑回收暂停状态下的Activity.
3. **停止状态**：当Activity完全不可见时，就进入了停止状态，系统仍然会为该Activity保留当前状态和成员信息，但是当内存不足时，该状态下的Activity会优先被回收。
4. **销毁状态**：Activity从返回栈中被移除后就变成了销毁状态。系统会最倾向于回收这种状态下的Activity，以保证内存充足。

## Activity生命周期中的七个回调方法

1. **onCreate()**：Activity第一次创建时被调用，可以在该方法中完成活动的初始化操作，如加载布局、绑定事件等。
2. **onStart()**：Activity不可见变为可见时调用，此时Activity还没有出现在前台，无法与用户进行交互。（Activity已经具有被用户看见的能力，但是因为还没有在前台，所以用户看不见）
3. **onResume()**：Activity可见并且已经在前台。此时的Activity位于栈顶，处于运行状态。
4. **onPause()**：系统准备去启动或者恢复另一个Activity时调用，当前Activity正在停止。可以在此方法做一些存储数据、停止动画的工作，但是不能在此做耗时操作，会影响到新的Activity的显示。（只有当onPause()执行完后，新Activity的onResume()才会执行）
5. **onStop()**：Activity即将停止，完全不可见时调用。可以做一些稍重量级的回收工作，但依然不能太耗时。
6. **onDestory()**：Activity被销毁之前调用，可以在此做一些回收工作和资源的释放。
7. **onRestart()**：Activity从停止状态变为运行状态时调用（不可见到可见），表示Activity正在重新启动。

![Activity的生命周期](D:\桌面\Activity的生命周期.png)



关于Dialog对话框的弹出：

Android组件Dialog的弹出不影响Activity的生命周期。

Theme为Dialog的Activity弹出会到导致原Activity调用onPause()，隐藏会调用onResume()



## Activity之间数据传递的方式：

1. Intent
   1. 传递基本数据类型
   2. 通过Bundle传递多种数据类型
   3. 通过Serializable和Parcelable传递对象
   4. startActivityForResult() + setResult()
2. 通过静态变量传递数据
3. 通过全局对象传递数据（Application）
   1. 可以使某些数据长时间驻留内存，以便程序随时调用
   2. 通过定义并注册Application类实现
4. 借用外部存储实现，如SharedPreferences，SQLite，File
5. LocalBroadcast，本地广播