mindmap
  root((core_service 学习总结))
    组件是什么
      电话服务子系统核心服务
      SystemAbility ID=4010
      开机启动常驻后台
    提供的能力
      SIM卡
        状态/文件/解锁
        单双卡/语音信箱
      搜网网络
        网络状态/信号
        选网/Radio/小区/5G
      eSIM
        APDU操作
      STK应急呼叫
    代码架构分层
      CoreService
        门面只分发
      Manager
        NetworkSearchManager
        SimManager
      Handler
        每卡槽一套
        维护缓存
      TelRilManager+TelRilXxx
        Request/Response/Notify
      RIL Adapter
        翻译官HDF服务
        跨进程
    启动流程
      ① CommonEventHub
      ② TelRilManager
        先连RIL Adapter
      ③ EsimManager
      ④ SimManager
      ⑤ NetworkSearchManager
      ⑥ CoreManagerInner注册
    核心链路
      数据在芯片AP侧缓存
      链路
        App
        CoreService
        Manager
        Handler
        TelRil
        RIL Adapter
        Modem
      请求链路
        InnerEvent造回执
        Request登记serialId
        跨进程调IRil
      返回链路
        响应按serialId认领
        主动上报Observer广播
      缓存机制
        接口读缓存
        芯片推送到缓存
    关键类
      TelRilManager
        连翻译官
      TelRilBase
        请求响应上报三件套
      TelRilCallback
        收翻译官回话
      TelRilRequest
        回执单
      ObserverHandler
        广播站
    核心术语
      AP主CPU
      Modem基带芯片
      RIL无线接口层
      RILAdapter翻译官
      HDI硬件设备接口
      HDF硬件驱动框架
      QMI AT指令
      IPC Binder
    Android RIL对照
      Android链路
        Framework
        rild HIDL
        QCRIL
        QMI
        Modem
      OHOS链路
        App
        CoreService
        tel_ril HDI
        RILAdapter
        Modem
      对应关系
        rild≈RILAdapter
        QCRIL≈RILAdapter内部
        ril_commands≈eventMap
      共同机制
        请求带序号路由
        主动上报广播
