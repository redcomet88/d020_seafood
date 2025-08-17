# d020_seafood
vue+flask海产品推荐与价格预测系统、双推荐+三种价格预测对比+知识图谱



> 完整源码关注学长微信: maimaidashuju   注明 github来的
> 


[video(video-X5gvFlBi-1755219429300)(type-bilibili)(url-https://player.bilibili.com/player.html?aid=113065114732432)(image-https://i-blog.csdnimg.cn/img_convert/c74fa540d20656001ba57b7b2a73ccea.jpeg)(title-D020 🦑vue+django海产品价格预测推荐知识图谱可视化系统（三种预测算法+终极炫酷界面）)]


编号: D020
技术架构: vue+flask+mysql+neo4j
核心技术：
- 基于用户和基于物品双推荐算法
- 线性回归、多项式回归、K临近三重价格预测算法，算法可视化比较
- echarts 可视化
- 知识图谱
- 爬虫更新数据

## 背景
随着数据科学技术的发展和农业信息化的深入推进，海产品价格预测系统成为了农业生产和经营管理中的重要工具。本系统基于Vue + Django的前后端分离架构，结合MySQL和Neo4j数据库，采用scikit-learn多项式回归算法进行价格预测，并利用协同过滤算法推荐海产品，旨在为用户提供准确的海产品价格预测和信息服务。系统不仅包括用户和管理员的常规管理功能，还引入了知识图谱和数据可视化技术，以增强系统的信息处理能力和用户体验。
## 1 系统介绍
海产品价格预测系统是一个集数据采集、处理、分析、预测于一体的综合信息服务平台。系统主要针对渔业领域的企业和个人，提供30多种渔业海产品的当前最新价格和历史价格数据，以及基于历史数据的价格预测服务。系统采用Vue + Django构建前后端分离架构，前端使用Vuetify框架提高开发效率和用户交互质量，后端以Django框架处理业务逻辑，数据存储则选用MySQL和Neo4j数据库，以适应不同的数据存储需求。
## 2 系统架构
前端技术：系统前端采用Vue.js框架和Vuetify组件库，构建了一个响应式、用户友好的界面。利用Vue的高效数据绑定和组件系统，配合Vuetify提供的丰富UI组件，为用户提供了流畅的操作体验。
后端技术：Django作为后端框架，不仅提供了强大的模型系统和自动的管理工具，还通过Django REST framework支持构建RESTful API，实现与前端的高效数据交互。
数据存储：系统采用MySQL和Neo4j双数据库设计。MySQL负责存储用户数据、海产品价格数据等结构化信息，而Neo4j用于构建领域的知识图谱，支持复杂的关系数据查询。
### 架构
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/779ba14c248b4534ba883a7f1f9acb18.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fd810ce4e4c54a48a0e518c3e4fabe76.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/922f82e4879c4bda80705ec858cd00fe.png)

### 需求
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b200c0ac0ca347c9ab0a0c6c255b2ce6.png)
### 技术报告
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f32e37eaff16485abd0fb755b6ae642c.png)
### 运行说明
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1ea9b1eb1d9b4d5ea926bf80fe92867b.png)
## 3 核心功能
数据采集与处理：系统能够自动采集30多种海产品的最新和历史价格信息，通过数据清洗和处理，保证数据质量和时效性。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4c32f6c79e6f4835aecab338a9f7ee18.png)

价格预测：采用scikit-learn的三种算法，根据农产品的历史价格数据进行价格预测，帮助用户预测未来价格变动趋势。
三种算法分别是： 线性回归、二次多项式回归、K临近算法， 可以看到图片中，不仅可以切换算法进行预测，还支持算法的可视化比较。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b03dab8b17934a4e9115625a6dab9bf2.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a66cedc57cee43b29c32330d73b516c6.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/11412d2718ab4170a0d2406d24fdff77.png)

```python
import numpy as np
import matplotlib.pyplot as plt
import pymysql

cnn = pymysql.connect(host=host, user=user, password=password, port=port, database=database,
                      charset='utf8')

###########1.数据生成部分##########
def f(x1, x2):
    y = 0.5 * np.sin(x1) + 0.5 * np.cos(x2) + 3 + 0.1 * x1
    return y


def load_data():
    x1_train = np.linspace(0, 50, 500)
    x2_train = np.linspace(-10, 10, 500)
    data_train = np.array([[x1, x2, f(x1, x2) + (np.random.random(1) - 0.5)] for x1, x2 in zip(x1_train, x2_train)],
                          dtype=object)
    x1_test = np.linspace(0, 50, 100) + 0.5 * np.random.random(100)
    x2_test = np.linspace(-10, 10, 100) + 0.02 * np.random.random(100)
    data_test = np.array([[x1, x2, f(x1, x2)] for x1, x2 in zip(x1_test, x2_test)])
    return data_train, data_test


def load_data_my():
    name = '肉蟹'
    sql = "select time1, avg(price) from tb_crop_price" \
          " where name='%s' group by time1    limit 12" % (name)
    with cnn.cursor() as cursor:
        cursor.execute(sql)

    print(sql)
    names = []

    y = []
    for line in cursor.fetchall():
        # print(line)
        y.append(line[1])
        names.append(line[0])

    y = np.array(y)
    x1_train = np.linspace(1, 12, 12)
    data_train = np.array([[x1, y] for x1, in zip(x1_train)], dtype=object)
    x1_test = np.linspace(1, 12, 12)
    data_test = np.array([[x1, y] for x1, in zip(x1_test)], dtype=object)
    return data_train, data_test

train, test = load_data_my()

# x_train, y_train = train[:, :2], train[:, 2]  # 数据前两列是x1,x2 第三列是y,这里的y有随机噪声
# x_test, y_test = test[:, :2], test[:, 2]  # 同上,不过这里的y没有噪声
x_train, y_train = train[:, :1], train[:, 1]  # 数据前两列是x1,x2 第三列是y,这里的y有随机噪声
x_test, y_test = test[:, :1], test[:, 1]  # 同上,不过这里的y没有噪声

###########2.回归部分##########
def try_different_method(model):
    model.fit(x_train, y_train)
    score = model.score(x_test, y_test)
    result = model.predict(x_test)
    plt.figure()
    plt.plot(np.arange(len(result)), y_test, 'go-', label='true value')
    plt.plot(np.arange(len(result)), result, 'ro-', label='predict value')
    plt.title('score: %f' % score)
    plt.legend()
    plt.show()


###########3.具体方法选择##########
####3.1决策树回归####
from sklearn import tree

model_DecisionTreeRegressor = tree.DecisionTreeRegressor()
####3.2线性回归####
from sklearn import linear_model

model_LinearRegression = linear_model.LinearRegression()
####3.3SVM回归####
from sklearn import svm

model_SVR = svm.SVR()
####3.4KNN回归####
from sklearn import neighbors

model_KNeighborsRegressor = neighbors.KNeighborsRegressor()
####3.5随机森林回归####
from sklearn import ensemble

model_RandomForestRegressor = ensemble.RandomForestRegressor(n_estimators=20)  # 这里使用20个决策树
####3.6Adaboost回归####
from sklearn import ensemble

model_AdaBoostRegressor = ensemble.AdaBoostRegressor(n_estimators=50)  # 这里使用50个决策树
####3.7GBRT回归####
from sklearn import ensemble

model_GradientBoostingRegressor = ensemble.GradientBoostingRegressor(n_estimators=100)  # 这里使用100个决策树
####3.8Bagging回归####
from sklearn.ensemble import BaggingRegressor

model_BaggingRegressor = BaggingRegressor()
####3.9ExtraTree极端随机树回归####
from sklearn.tree import ExtraTreeRegressor

model_ExtraTreeRegressor = ExtraTreeRegressor()
###########4.具体方法调用部分##########
try_different_method(model_ExtraTreeRegressor)

```

推荐算法：系统集成了两种协同过滤推荐算法，根据用户的浏览和购买行为，推荐潜在感兴趣的农产品。

数据可视化：使用ECharts进行数据可视化，包括大屏展示、果品价格分析等功能，直观展现农产品价格趋势和分析结果。
可视化大屏：针对果品价格分析和其他农产品的数据可视化需求，系统设计了可视化大屏功能，通过图表和图形直观展示数据分析结果，增强用户体验。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8e6f8e116bb5487b945f3c527c29e98e.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c17e8215a219463ba413666a8606e3c3.png)

用户和管理员管理：完整的用户管理功能，包括注册、登录、信息修改、短信验证码修改密码等，管理员则具有权限管理功能。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/df038316d7dc4770b8f3208dccf74ab9.png)

知识图谱：创建农业领域的知识图谱，为用户提供丰富的农业知识和信息。知识图谱的引入，不仅丰富了系统的数据维度，还提高了数据分析和推荐的准确性。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3a2f7fcfbf164ad6a1285e2ac0e7b09e.png)
管理员可以使用权限和用户管理功能
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ca2cac194c754736a68e79cffcb0f328.png)
登录、注册和个人设置
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c653aaa70f324b0d8e3e72e0b0898694.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b590148896cf4746b2449c0163e2ee13.png)

## 4 实现难点与解决方案
数据采集的准确性和时效性：系统通过定时任务自动采集数据，并结合人工审核机制，确保数据的准确性和更新及时。
价格预测的准确度：选择适合的多项式回归模型并进行细致的参数调优，通过历史数据训练确保预测结果的可靠性。
知识图谱的构建和应用：采用专业的农业知识进行知识图谱的构建，并通过Neo4j数据库高效管理和查询复杂的关系数据。
## 5 数据获取与处理
爬取数据
系统的数据采集核心依赖于对中国农产品交易信息网（pfsc.agri.cn）的实时监控与数据抓取。通过编写高效的数据爬虫程序，使用Python的requests库直接向目标网站的API发送请求，获取农产品的最新及历史价格数据。返回的数据格式为JSON，便于进一步的处理和分析。我们的数据处理流程包括数据清洗、去重、格式化，最终将整理好的数据存储到MySQL数据库中，以供后续的数据分析和价格预测使用。
此外，系统设计了一套灵活的数据更新机制，确保无需修改代码即可定期自动更新数据，并且实时展示到用户界面。这一机制不仅大大降低了系统维护的工作量，也保证了用户总能获取到最新的市场价格信息。
需要注意的是，系统中农产品的图片是我们自己去网上找之后放在系统中的，如果新增农产品，图片是需要自己去找到的。
## 结论
海产品价格预测系统的建立为农业生产和市场分析提供了有力的技术支持。未来，我们计划引入更多的数据源和先进的预测模型，进一步提高价格预测的准确度。同时，将探索利用机器学习和人工智能技术，优化推荐算法，提升用户个性化服务的质量。此外，系统将继续丰富知识图谱内容，拓展可视化分析功能，以满足用户更为多样化的信息需求。
