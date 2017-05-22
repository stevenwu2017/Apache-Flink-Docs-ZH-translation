---
title: "Twitter Connector"
nav-title: Twitter
nav-parent_id: connectors
nav-pos: 8
---
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

   Twitter Streaming API 提供了,Twitter提供的对Twitter流的访问。Flink Streaming自带一个内置`TwitterSource`类, 来建立一个到这个Twitter流的连接。若要使用此连接器，请将下列依赖项添加到项目中：

{% highlight xml %}
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-connector-twitter{{ site.scala_version_suffix }}</artifactId>
  <version>{{site.version }}</version>
</dependency>
{% endhighlight %}

   注意，流连接器目前不是二进制发布包的一部分。在集群中执行时,请看链接， [这里]({{site.baseurl}}/dev/linking.html).

#### 认证
   为了连接到Twitter流，用户必须在应用程序中注册他们，并获得必要的信息进行身份验证。该过程如下所述。

#### 获取认证信息
   首先，需要一个Twitter帐户。免费注册在 [twitter.com/signup](https://twitter.com/signup)
或在Twitter的 [应用管理](https://apps.twitter.com/) 里登录和登记程序通过点击“创建新的应用程序”按钮。
填写一份关于你的计划的表格,并接受条款和条件。在选择这个应用之后，API密钥和API的机密
(在`TwitterSource`里分别称为 `twitter-source.consumerKey` 和 `twitter-source.consumerSecret`) 位于“API密钥”选项卡。
必要的OAuth访问令牌的数据 (`TwitterSource`包含`twitter-source.token` 和 `twitter-source.tokenSecret`) 可以产生和获得,
在“密钥和访问令牌”选项卡。记住把这些信息保密，不要把它们推送到公共仓库。

#### 用法
   对比其他连接器，该`TwitterSource`不依赖于额外的服务。例如下面的代码应该运行优雅：

<div class="codetabs" markdown="1">
<div data-lang="java" markdown="1">
{% highlight java %}
Properties props = new Properties();
props.setProperty(TwitterSource.CONSUMER_KEY, "");
props.setProperty(TwitterSource.CONSUMER_SECRET, "");
props.setProperty(TwitterSource.TOKEN, "");
props.setProperty(TwitterSource.TOKEN_SECRET, "");
DataStream<String> streamSource = env.addSource(new TwitterSource(props));
{% endhighlight %}
</div>
<div data-lang="scala" markdown="1">
{% highlight scala %}
val props = new Properties();
props.setProperty(TwitterSource.CONSUMER_KEY, "");
props.setProperty(TwitterSource.CONSUMER_SECRET, "");
props.setProperty(TwitterSource.TOKEN, "");
props.setProperty(TwitterSource.TOKEN_SECRET, "");
DataStream<String> streamSource = env.addSource(new TwitterSource(props));
{% endhighlight %}
</div>
</div>

   `TwitterSource` 发射一个包含JSON对象的字符串,代表一个Tweet。
   `TwitterExample` 类在 `flink-examples-streaming` 包里,展示了一个如何使用 `TwitterSource` 完整的例子。
默认情况下，该 `TwitterSource` 使用 `StatusesSampleEndpoint`。 这个端点返回一个随机样本的Tweets。
有一个 `TwitterSource.EndpointInitializer` 接口,允许用户提供自定义的端点。
