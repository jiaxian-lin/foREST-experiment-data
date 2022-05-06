

# foREST



foREST 是一款基于OpenAPI文档的有状态的REST API模糊测试工具，用于通过REST API自动测试云服务并查找这些服务中的安全性和可靠性错误。foREST会分析OpenAPI文档，然后生成并执行测试用例，从而对该云服务进行测试。

foREST会依据OpenAPI文档，自动地推断出云服务API之间的生产者-消费者关系，构建依赖树，并依据依赖树生成满足依赖关系的测试用例，这种方式使得foREST能够智能地生成测试用例，提高测试用例的生成效率，并发现更多的错误。


## foREST代码结构

foREST代码总体分为两部分：[依赖分析](https://github.com/Artisan-Lab/Restful-api-testing/blob/FoREST_copy/dependency/dependency.md) 与 [测试执行](https://github.com/Artisan-Lab/Restful-api-testing/blob/FoREST_copy/testing_render/testing.md) 。



## 使用说明



1.将待测服务的yaml文档存放在openapi文件夹下

2.根据实际需要在配置文件[FoREST_config](https://github.com/Artisan-Lab/Restful-api-testing/blob/FoREST_copy/FoREST_config.conf)中配置

3.安装运行foREST所需的依赖
```bash
pip3 install -r requirements.txt
```

启动命令
```bash
python3 foREST.py
```

 ## 实验设置

具体实验过程见[实验文件](https://github.com/Artisan-Lab/Restful-api-testing/blob/FoREST_copy/experiment.md)


 
## BUG

| Date | Project | Link | Finder | Status | Description |
|---------|---------|---------|---------|---------|---------|
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) |  | submitted | POST  /hooks |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) |  | submitted | POST  /admin/clusters/add |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) |  | submitted | POST  /clusters/{id}/metrics_dashboard/annotations/ |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |  | submitted | DELETE/PUT/GET  /users/{id}/custom_attributes/{key} |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |  | submitted | GET  /users/{id}/custom_attributes |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) |  | submitted | POST  /projects/{id}/clusters/user |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) |  | submitted | POST  /projects/{id}/metrics/user_starred_dashboards |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |  | submitted | DELETE/POST  /projects/{id}/custom_attributes/{key} |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) |  | submitted | POST  /projects/{id}/export | 
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |  | submitted | GET  /projects/{id}/custom_attributes |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) |  | submitted | POST  /groups/{id}/clusters/user |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |  | submitted | GET /groups/{id}/custom_attributes |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |  | submitted | DELETE/PUT/GET  /groups/{id}/custom_attributes/{key} |
| 2021-7-4 | WordPress |  |  | unsubmitted | POST  /categories |
| 2021-11-4| Gitlab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346563) |  | submitted | POST projects/{id}/fork/forked_from_id |


 

