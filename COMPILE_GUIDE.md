# RoadSurvey 完整编译和部署指南

## 项目状态
✅ **项目框架完成** - 所有核心代码和布局文件已创建

## 文件清单

### 已完成的主要代码文件
- ✅ app/build.gradle.kts（包含所有依赖）
- ✅ settings.gradle.kts
- ✅ AndroidManifest.xml
- ✅ 5个数据模型（User, Project, Bridge, Obstacle, OtherObstacle）
- ✅ 5个DAO数据访问对象
- ✅ Room数据库主类
- ✅ 7个Activity（登录、主菜单、各数据模块、报告生成）
- ✅ 3个RecyclerView Adapter
- ✅ 4个工具类（加密、压缩、更新检查、Word生成）
- ✅ 15个XML布局文件
- ✅ 资源文件（strings, colors, themes, arrays）

## 快速开始指南

### 前置条件
1. **Android Studio** 2022.1 或更新版本
2. **JDK** 11或更高版本
3. **Android SDK** 已安装API 34

### 步骤1：导入项目

```bash
# 在Android Studio中
File → Open → 选择RoadSurvey项目目录
```

### 步骤2：同步Gradle

```
Android Studio会自动开始同步，等待完成
或手动运行: Build → Make Project
```

### 步骤3：运行应用

**方式A：通过Android Studio**
```
1. 连接真实设备或启动模拟器
2. Run → Run 'app'
3. 或按快捷键 Shift + F10
```

**方式B：通过命令行**
```bash
# 连接设备或启动模拟器
adb devices

# 编译APK
./gradlew clean build

# 安装应用
./gradlew installDebug

# 或直接运行
./gradlew assembleDebug
```

### 步骤4：首次登录

**默认凭证：**
- 用户名：user
- 密码：123456

## 项目结构说明

```
RoadSurvey/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── kotlin/com/cosco_km/roadsurvey/
│   │   │   │   ├── data/
│   │   │   │   │   ├── dao/          # 数据访问对象
│   │   │   │   │   ├── database/     # Room数据库
│   │   │   │   │   └── model/        # 数据模型
│   │   │   │   ├── ui/
│   │   │   │   │   ├── adapter/      # RecyclerView适配器
│   │   │   │   │   ├── *Activity.kt  # 各个界面
│   │   │   │   └── util/             # 工具类
│   │   │   ├── res/
│   │   │   │   ├── layout/           # XML布局文件
│   │   │   │   ├── values/           # 字符串、颜色、样式
│   │   │   │   ├── xml/              # 其他XML配置
│   │   │   │   └── mipmap/           # 应用图标
│   │   │   └── AndroidManifest.xml
│   │   └── test/                     # 单元测试
│   └── build.gradle.kts
├── build.gradle.kts
├── settings.gradle.kts
└── README.md
```

## 核心功能说明

### 1. 登录系统
- **位置**: LoginActivity.kt
- **功能**: 用户认证，密码加密验证
- **管理员设置**: 修改登录密码

### 2. 项目管理
- **位置**: ProjectInfoActivity.kt
- **功能**: 创建和编辑项目基本信息
- **包含**: 项目名称、货物参数、车辆配置、勘测人员、时间、起点、终点等

### 3. 数据录入
- **桥梁数据** (BridgeDataActivity.kt)
- **限高/涵洞** (ObstacleDataActivity.kt)
- **其他障碍** (OtherObstacleActivity.kt)
- **共同特性**: 支持多张照片、K值排序、编辑和删除

### 4. Word报告生成
- **位置**: WordGenerationActivity.kt
- **功能**: 自动生成完整的勘测报告
- **包含**:
  - 概述表
  - 桥梁统计表
  - 限高杆、涵洞统计表
  - 其他障碍统计表
  - 道路情况详细描述表
  - 所有照片自动嵌入

### 5. 数据存储
- **数据库**: SQLite (通过Room ORM)
- **存储位置**: `/data/data/com.cosco_km.roadsurvey/`
- **文件存储**: 手机内部存储 → `getExternalFilesDir()`
- **输出文件**: 下载文件夹 (`/Download/`)

## 关键技术细节

### 密码加密
```kotlin
// SHA-256 + Salt
val salt = "cosco_km_roadsurvey_salt_2024"
MessageDigest.getInstance("SHA-256")
    .digest(salt + password)
```

### 照片压缩
```kotlin
// 自动压缩 >1MB 的图片到 1MB
// 方式: 先降低质量，再缩放尺寸
```

### 数据排序
```kotlin
// 按K值数值部分排序
// K31 → 31.0
// K3.5 → 3.5
// 自动提取并排序
```

### Word生成
```kotlin
// 使用Apache POI库
// 自动按K值混合三种数据
// 嵌入式照片（JPEG格式）
```

## 依赖管理

### 主要依赖
```gradle
// Core
androidx.appcompat:appcompat:1.6.1
com.google.android.material:material:1.11.0

// Database
androidx.room:room-runtime:2.6.1

// Word处理
org.apache.poi:poi:5.0.0
org.apache.poi:poi-ooxml:5.0.0

// 图像处理
id.zelory:compressor:3.0.1

// 网络
com.squareup.okhttp3:okhttp:4.11.0

// JSON
com.google.code.gson:gson:2.10.1

// 加密
androidx.security:security-crypto:1.1.0-alpha06
```

## 常见编译问题和解决方案

### 问题1: Gradle同步失败
**解决方案**:
```bash
# 清除缓存
./gradlew clean

# 重新同步
./gradlew sync
```

### 问题2: Apache POI 依赖冲突
**解决方案**:
```gradle
// 在build.gradle中添加
configurations {
    all {
        resolutionStrategy {
            force 'org.apache.xmlbeans:xmlbeans:5.0.0'
        }
    }
}
```

### 问题3: 照片权限问题
**解决方案**:
```
确保在AndroidManifest.xml中有以下权限:
- android:name="android.permission.READ_EXTERNAL_STORAGE"
- android:name="android.permission.WRITE_EXTERNAL_STORAGE"
- android:name="android.permission.CAMERA"
```

### 问题4: Word生成失败
**解决方案**:
1. 检查存储空间是否充足
2. 检查下载文件夹是否可写
3. 查看Logcat日志获取详细错误信息

## 调试技巧

### 查看数据库
```bash
# 通过Android Studio Database Inspector
View → Tool Windows → Database Inspector
```

### 查看日志
```bash
# 实时查看
./gradlew logcat

# 或通过Android Studio Logcat窗口
```

### 模拟器配置
```
推荐配置:
- API 30或以上
- RAM: 4GB+
- Storage: 2GB+
```

## 应用打包和发布

### 生成Debug APK
```bash
./gradlew assembleDebug
# 输出: app/build/outputs/apk/debug/app-debug.apk
```

### 生成Release APK
```bash
# 需要配置keystore
./gradlew assembleRelease
```

### 生成AAB（推荐用于Google Play）
```bash
./gradlew bundleRelease
```

## 性能优化建议

1. **数据库查询优化**
   - 使用索引加速排序
   - 使用分页加载大数据集

2. **图像处理优化**
   - 已实现自动压缩
   - 考虑缓存机制

3. **内存管理**
   - 及时释放Bitmap资源
   - 使用WeakReference处理临时对象

## 安全性考虑

1. ✅ 密码加密存储（SHA-256 + Salt）
2. ✅ 本地数据加密（建议使用SQLCipher）
3. ✅ 文件访问权限控制
4. ✅ 输入数据验证

## 测试清单

- [ ] 登录功能测试
- [ ] 项目创建和编辑测试
- [ ] 数据录入和验证测试
- [ ] 照片上传和压缩测试
- [ ] 数据排序和编号测试
- [ ] Word报告生成测试
- [ ] 数据删除和编辑测试
- [ ] 离线模式测试
- [ ] 权限申请测试
- [ ] 边界值测试

## 下一步开发计划

### 短期（V1.1）
1. 添加数据备份功能
2. 实现数据导出为CSV
3. 添加更新检查实现

### 中期（V2.0）
1. 云端同步功能
2. 多用户支持
3. 离线地图集成
4. PDF导出

### 长期（V3.0）
1. 实时GPS定位
2. AR勘测辅助
3. 数据分析仪表板
4. Web管理端

## 维护和支持

### 获取帮助
1. 查看README.md
2. 查看代码注释
3. 检查Logcat输出
4. 联系开发团队

### 报告问题
- 详细描述问题
- 提供日志信息
- 包含重现步骤
- 提供设备信息

## 许可证
© 2024 COSCO-KM. All rights reserved.

---

**项目完成日期**: 2024年
**最后更新**: 2024年6月19日
**版本**: 1.0.0
