# 定时任务列表

## 触发性定时任务

### CloneCourseSetJob

#### 描述

整个课程复制的定时任务

#### 参数


#### 使用

任务创建后即执行，无超时时间


### CourseDataCleanJob

#### 描述

更新课程学员学习数据计数

#### 参数


#### 使用

edusoho 8.0.24 版本升级后执行，执行完之后立即结束任务，无超时时间

### RefreshAllCourseTaskSeqJob

#### 描述

更新所有任务的顺序

#### 参数


#### 使用

edusoho 8.1.2 8.1.3 升级后执行，执行完之后立即结束任务，无超时时间


## 常驻任务

### RefreshLearningProgressJob

#### 描述

刷新学员学习进度数据

#### 参数

#### 使用

每天执行一次（2点） 无超时时间

### UpdateCourseSetHotSeqJob

#### 描述

更新近一段时间的课程热度排序

#### 参数


#### 使用

每天执行一次 无超时时间
