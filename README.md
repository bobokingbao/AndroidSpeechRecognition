# AndroidSpeechRecognition
基于 TensorFlow Lite 的 Android 端中文语音识别 Demo。

声学模型来源于：[ASRT_SpeechRecognition](https://github.com/nl8590687/ASRT_SpeechRecognition "基于深度学习的中文语音识别系统")
<br>
使用的模型版本：[ASRT version 0.4.2 released](https://github.com/nl8590687/ASRT_SpeechRecognition/releases/tag/v0.4.2 "ASRT version 0.4.2 released")


## 系统结构/类的说明

* **MainActivity**<br>
为 Android 程序入口，初始化 UI 控件与其监听函数所在，肩负生成 TensorFlowInferenceInterface 对象与读取标签文件列表的任务、避免重复读取文件。

* **DreamCarcher**<br>
调用网络开源 api、其中通过 Android 官方提供的 AudioCapturer 提供录音功能与文件流，默认地将语音数据保存至手机指定目录下 farewell.wav 处。

* **Chain**<br>
调用 InitialDream 类的对象以获取处理好的语音数据，输入模型以得到返还的概率矩阵，通过 Argmax 函数识别出拼音序列下标后，传入标签文件列表得到最终结果，返还给 MainActivity 的 TextView 控件以显示于应用界面。

* **InitialDream**<br>
为数据读取与处理功能提供支持，在 Chian 的预测函数启动时会自动地初始化并运行，内部实现对 Wave 文件的读取、对其数据进行加窗处理，最终生成模型所需识别输入的操作。

* **AnnaUtil**<br>
包含 4 种静态变量与 4 种静态函数，前者用于提供录音标准帧速率与文件路径，后者包含对复数求模、对数组求 log(n+1)，整型数据转浮点数和一维数组内求 Max 数值下标，协助上述各类进行工作。


## APP 使用说明
通过下拉控件可选择测试已存在的 thchs30 train/test 数据集文件，用于数据集测试的 wave 文件需要手动放置于手机指定目录（也可使用其它语音文件，请依照 AnnaUtil 内变量修改语音文件名/修改功能对应变量）。<br><br>
测试语句将显示范例句、识别结果并提供错误率检测；实时录音仅显示识别结果。<br><br>
按 START 按钮开始测试，按 STOP 按钮结束录音、查看语音识别结果，按 PLAY 按钮播放对应语音文件。


## 注意
* 该项目存在许多功能性与代码书写规范问题<br><br>
不支持识别时长过长（>1600）的语音数据。<br><br>
暂无拼音译为汉字功能，需要语言模型模块。<br><br>
生成 TensorFlowInferenceInterface 对象与读取标签文件列表的任务，应当交由单独的类来处理。
