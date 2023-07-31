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


<div class="col-lg-4 mb-5 mb-lg-0 text-center">
    <div>
        <i><img src="npartword_logo-removebg.png" style="width:150px;height:150px"></i>
    </div>
    <h4 class="h3">
        npartword
    </h4>
    <div class="mb-0">
        <a href="https://github.com/HughNian/npartword" style="color:#bdd7fe">npartword</a>中文分词服务，使用 <b>nmid</b> 作为worker可以向外部提供分词服务。
    </div>
</div>

<div class="col-lg-4 mb-5 mb-lg-0 text-center">
    <div>
        <i><img src="nsearch_logo-removebg.png" style="width:150px;height:150px"></i>
    </div>
    <h4 class="h3">
        nsearch
    </h4>
    <div class="mb-0">
        <a href="https://github.com/HughNian/nsearch" style="color:#bdd7fe">nsearch</a>全文搜索服务，使用 <b>nmid</b>作为worker可以向外部提供全文搜索服务，同时通过<b>nmid</b>使用npartword的worker的分词服务。
    </div>
</div>

<div class="col-lg-4 mb-5 mb-lg-0 text-center">
    <div>
        <i><img src="nmid-frame_logo-removebg.png" style="width:350px;height:150px"></i>
    </div>
    <h4 class="h3">
        nmid-frame
    </h4>
    <div class="mb-0">
        <a href="https://github.com/nmid-team/goframe" style="color:#bdd7fe">nmid-frame</a>是nmid的golang应用开发框架，可以方便迅速的开发基于<b>nmid</b>的web应用或者是独立<b>nmid</b>worker服务。
    </div>
</div>

{{% /blocks/section %}}
