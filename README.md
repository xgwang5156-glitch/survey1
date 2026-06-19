# RoadSurvey 道路勘测系统 - 完整项目指南

## 项目概述
这是一个Android原生应用（Kotlin），用于大件运输的道路勘测数据管理和Word报告生成。

## 项目信息
- **应用包名**: com.cosco_km.roadsurvey
- **最低SDK**: Android 8.0 (API 26)
- **目标SDK**: Android 14 (API 34)
- **版本**: 1.0

## 已完成的核心代码

### 1. 项目配置
- ✅ settings.gradle.kts - 项目根配置
- ✅ app/build.gradle.kts - 应用级配置，包含所有依赖
- ✅ AndroidManifest.xml - 应用清单

### 2. 数据模型 (app/src/main/kotlin/com/cosco_km/roadsurvey/data/model/)
- ✅ User.kt - 用户模型
- ✅ Project.kt - 项目模型
- ✅ Bridge.kt - 桥梁模型
- ✅ Obstacle.kt - 限高/涵洞模型
- ✅ OtherObstacle.kt - 其他障碍模型

### 3. 数据库 (app/src/main/kotlin/com/cosco_km/roadsurvey/data/)
- ✅ dao/UserDao.kt
- ✅ dao/ProjectDao.kt
- ✅ dao/BridgeDao.kt
- ✅ dao/ObstacleDao.kt
- ✅ dao/OtherObstacleDao.kt
- ✅ database/RoadSurveyDatabase.kt - Room数据库主类

### 4. 工具类 (app/src/main/kotlin/com/cosco_km/roadsurvey/util/)
- ✅ EncryptionUtil.kt - 密码加密工具
- ✅ ImageCompressionUtil.kt - 照片压缩工具
- ✅ UpdateChecker.kt - 更新检查工具
- ✅ WordGenerator.kt - Word报告生成核心引擎

### 5. Activity (app/src/main/kotlin/com/cosco_km/roadsurvey/ui/)
- ✅ LoginActivity.kt - 登录页面
- ✅ MainActivity.kt - 主菜单页面
- ✅ ProjectInfoActivity.kt - 项目基础信息
- ✅ BridgeDataActivity.kt - 桥梁数据管理
- ✅ ObstacleDataActivity.kt - 限高/涵洞管理
- ✅ OtherObstacleActivity.kt - 其他障碍管理
- ✅ WordGenerationActivity.kt - Word报告生成

### 6. Adapter (app/src/main/kotlin/com/cosco_km/roadsurvey/ui/adapter/)
- ✅ BridgeAdapter.kt
- ✅ ObstacleAdapter.kt
- ✅ OtherObstacleAdapter.kt

### 7. 资源文件
- ✅ xml/file_paths.xml - FileProvider配置
- ✅ values/strings.xml - 字符串资源
- ✅ layout/activity_login.xml - 登录界面

## 需要完成的布局文件

### 布局文件清单 (app/src/main/res/layout/)
需要创建以下XML布局文件：

1. **activity_main.xml** - 主菜单界面
   - 五个导航按钮：项目信息、桥梁数据、限高/涵洞、其他障碍、生成Word
   - 显示当前项目名称
   - 退出登录按钮

2. **activity_project_info.xml** - 项目基础信息
   - 项目名称输入框
   - 货物参数输入框
   - 车辆配置输入框
   - 勘测人输入框
   - 勘测时间（日期选择器）
   - 起点/终点输入框
   - 距离输入框
   - 备注输入框
   - 物流环境总结（多行文本）
   - 路线图上传按钮
   - 保存按钮

3. **activity_bridge_data.xml** - 桥梁数据
   - 桥梁名称、K值、道路编号、跨距、宽度等输入框
   - 下拉选择：道路编号、桥梁类型、改造方式
   - 照片上传按钮（显示已选张数）
   - 清除照片按钮
   - 保存按钮
   - RecyclerView列表显示已添加的桥梁

4. **activity_obstacle_data.xml** - 限高/涵洞数据
   - 类似activity_bridge_data.xml的结构
   - 特定字段：障碍类型、障碍名称、最低高度
   - 下拉选择：改造方式（拆除后通行、临时升高后通行）

5. **activity_other_obstacle.xml** - 其他障碍数据
   - K值、障碍名称、道路编号
   - 道路高差输入框（默认0.2-0.4m）
   - 照片和保存功能

6. **activity_word_generation.xml** - 报告生成
   - 生成报告按钮
   - 进度条
   - 状态文本
   - 文件路径显示

7. **dialog_admin_settings.xml** - 管理员设置对话框
   - 管理员密码输入框
   - 新密码输入框
   - 确认和取消按钮

8. **item_bridge.xml** - 桥梁列表项
   - 显示桥梁名称、K值、道路、类型、改造方式
   - 编辑和删除按钮

9. **item_obstacle.xml** - 障碍列表项
   - 显示障碍信息
   - 编辑和删除按钮

10. **item_other_obstacle.xml** - 其他障碍列表项
    - 显示障碍信息
    - 编辑和删除按钮

## 资源文件需求

### values/colors.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>
```

### values/themes.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.RoadSurvey" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorError">@android:color/holo_red_light</item>
        <item name="colorOnPrimary">@color/white</item>
        <item name="colorOnSecondary">@color/black</item>
        <item name="colorOnBackground">@color/black</item>
        <item name="colorOnSurface">@color/black</item>
    </style>
</resources>
```

## 关键功能说明

### 1. 密码管理
- 默认用户：user
- 默认密码：123456（加密存储）
- 使用SHA-256哈希+盐值加密
- 管理员可以修改密码

### 2. 数据存储
- 使用SQLite数据库（Room ORM）
- 支持离线数据存储
- 自动按K值排序

### 3. 照片处理
- 支持每条数据最多3张照片
- 自动压缩>1MB的照片到1MB
- 嵌入式保存在Word中

### 4. Word生成
- 自动生成完整的勘测报告
- 包含4个统计表和1个详细描述表
- 所有数据自动按K值排序
- 照片自动嵌入文档

### 5. 更新检查
- 内置检查更新功能
- 可配置更新服务器地址

## 编译和运行步骤

### 前置条件
1. Android Studio 2022.1 或更新版本
2. JDK 1.8 或更新版本
3. Android SDK API 34

### 编译步骤
1. 在Android Studio中打开项目
2. 等待Gradle依赖下载完成
3. 点击"Build" > "Make Project"
4. 连接Android设备或启动模拟器
5. 点击"Run" > "Run 'app'"

### 默认登录凭证
- 用户名: user
- 密码: 123456

## 数据库架构

### users表
- id: 主键
- username: 用户名
- encryptedPassword: 加密密码
- isAdmin: 是否管理员
- createdAt: 创建时间

### projects表
- id: 主键
- projectName: 项目名称
- cargoParams: 货物参数
- vehicleConfig: 车辆配置
- surveyor: 勘测人
- surveyDate: 勘测日期
- startPoint: 起点
- endPoint: 终点
- distance: 距离
- remarks: 备注
- logisticsSummary: 物流环保总结
- routeMapPath: 路线图路径
- createdAt/updatedAt: 时间戳

### bridges/obstacles/other_obstacles表
- id: 主键
- projectId: 外键关联项目
- 各自特定的字段
- photoPaths: JSON格式的照片路径列表

## 文件结构
```
RoadSurvey/
├── app/src/main/
│   ├── kotlin/com/cosco_km/roadsurvey/
│   │   ├── data/
│   │   │   ├── dao/
│   │   │   ├── database/
│   │   │   └── model/
│   │   ├── ui/
│   │   │   ├── adapter/
│   │   │   └── *.Activity.kt
│   │   └── util/
│   ├── res/
│   │   ├── layout/
│   │   ├── values/
│   │   ├── xml/
│   │   └── mipmap/
│   └── AndroidManifest.xml
├── build.gradle.kts
└── settings.gradle.kts
```

## 常见问题

### 1. 编译错误
- 确保所有依赖版本兼容
- 清理并重新构建项目
- 检查Java版本（推荐JDK 11+）

### 2. 数据库迁移
- 当数据库结构改变时，增加version字段
- 实现Migration类处理数据迁移

### 3. 照片压缩问题
- 如果压缩失败，检查存储空间
- 确保已授予文件读写权限

### 4. Word生成失败
- 检查项目是否有数据
- 确保下载文件夹可写
- 查看日志获取详细错误信息

## 下一步开发

### 建议的功能扩展
1. 实现云同步功能
2. 添加多语言支持
3. 导出PDF功能
4. 数据备份和恢复
5. 离线地图集成

## 技术栈

- **语言**: Kotlin
- **框架**: Android SDK, AndroidX
- **数据库**: Room (SQLite)
- **UI**: Material Design
- **文档生成**: Apache POI
- **图像处理**: Android Bitmap
- **网络**: OkHttp, Gson
- **密码安全**: SHA-256
- **异步处理**: Coroutines

## 许可证
© 2024 COSCO-KM. All rights reserved.

## 联系方式
如有问题或建议，请联系开发团队。
