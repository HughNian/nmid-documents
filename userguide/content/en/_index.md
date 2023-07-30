---
title: nmid
description: A golang micro service framework
---

{{% blocks/cover title="Welcome to nmid" height="full" %}}
{{% param description %}}
{.display-6}

<a class="btn btn-lg btn-primary me-3" href="about/">Learn More</a>
<a class="btn btn-lg btn-secondary" href="https://github.com/HughNian/nmid/releases" target="_blank">Get started</a>
{.p-initial .my-5}

<!-- <span style="margin-top:25px;margin-bottom:15px">
<a class="github-button" href="https://github.com/HughNian/nmid" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star nmid">Star nmid</a>
<a class="github-button" href="https://github.com/HughNian/nmid-c" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star nmid-c">Star nmid-c</a>
<a class="github-button" href="https://github.com/HughNian/nmid-php-ext" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star nmid-php-ext">Star nmid-php-ext</a>
</span> -->


{{% blocks/link-down color="info" %}}
{{% /blocks/cover %}}

{{% blocks/lead color="1" %}}
👏👏Golang微服务框架、rpc框架、k8s sidecar、serverless函数  

🤟🤟nmid意思为中场指挥官，足球场上的中场就是统领进攻防守的核心。这里是服务的调度核心。一个轻量级分布式微服务RPC框架。 
{{% /blocks/lead %}}

{{% blocks/section color="dark" type="row" %}}

{{% blocks/feature icon="fa-brands fa-github" title="nparword" %}}
[nparword](https://github.com/HughNian/npartword)中文分词服务，使用 **nmid** 作为worker可以向外部提供分词服务。
{{% /blocks/feature %}}


{{% blocks/feature icon="fa-brands fa-github" title="nsearch" %}}
[nsearch](https://github.com/HughNian/nsearch)全文搜索服务，使用 **nmid** 作为worker可以向外部提供全文搜索服务，同时使用**nmid**npartword的worker的分词服务，作为搜索时所用分词功能。
{{% /blocks/feature %}}


{{% blocks/feature icon="fa-brands fa-github" title="nmidframe" %}}
[nmid-frame](https://github.com/nmid-team/goframe)是nmid的golang应用开发框架，可以方便迅速的开发基于**nmid**的web应用或者是独立**nmid**worker服务。
{{% /blocks/feature %}}


{{% /blocks/section %}}
