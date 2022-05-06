# foREST-experiment-data
This is the companion repo for the paper **foREST: A Tree-based Approach for Fuzzing RESTful APIs** submitted to ASE 2022.


`source code`:contains the tools described in the paper(Contains how to reproduce the experiments mentioned in the paper）


A summary of the contents of this page is as follows:
1. Summary of Bugs found by foREST: This part shows the summary of bugs found by foREST
2. Simpel introduction of bugs: This part shows the bugs found by foREST during testing
3. Steps to reproduce the bugs: This part shows how to reproduce the bugs



## simple introduction of bugs
We roughly divide the bugs we find into three categories
| id | classification                    | Server    | Endpoint                                                                | Method             | issue                                                         |
|----|-----------------------------------|-----------|-------------------------------------------------------------------------|--------------------|---------------------------------------------------------------|
| 1  | Logical: use after delete                  | GitLab    | /users/{id}/custom_attributes /users/{id}/custom_attributes/{key}       | GET GET/PUT/DELETE | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |
| 2  | Logical: use after delete                  | GitLab    | /projects/{id}/custom_attributes /projects/{id}/custom_attributes/{key} | GET GET/PUT/DELETE | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |
| 3  | Logical: use after delete                  | GitLab    | /groups/{id}/custom_attributes /groups/{id}/custom_attributes/{key}     | GET GET/PUT/DELETE | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) |
| 4  | Logical: double delete | GitLab    | /projects/{id}/services/github                                          | DELETE             | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360147) |
| 5  | invalid parameter: UTF-8                | GitLab    | /hooks                                                                  | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) |
| 6  | invalid parameter: UTF-8                 | GitLab    | /projects/{id}/metrics/user_starred_dashboards                          | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) |
| 7  | invalid parameter: UTF-8                 | GitLab    | /admin/cluster/add                                                      | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) |
| 8  | invalid parameter: UTF-8                | GitLab    | /projects/{id}/cluster/user                                             | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) |
| 9  | invalid parameter: UTF-8                | GitLab    | /groups/{id}/cluster/user                                               | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) |
| 10 | invalid parameter: UTF-8                 | GitLab    | /projects/{id}/export                                                   | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) |
| 11 | invalid parameter: special characters                 | GitLab    | /projects/{project_id}/variables/{key}                                  | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360662) |
| 12 | invalid parameter: enum type with bad value              | GitLab    | /projects/{id}/environments                                             | GET                | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360138) |
| 13 | invalid parameter: long str with special char  | GitLab    | /projects/{id}/repository/commits                                       | GET                | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/356922) |
| 14 | invalid parameter: special characters       | GitLab    | /projects/{id}/repository/commits                                       | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360312) |
| 15 | logical: false logic                        | GitLab    | /projects/{id}/repository/branches.                                     | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360313) |
| 16 | logical: reference loop                        | GitLab    | /projects/{id}/fork/{forked_from_id}                                    | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346563) |
| 17 | unsuported function                   | GitLab    | /projects                                                               | POST               | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/356921) |
| 18 | unsuported function                  | WordPress | /categories/{id}                                                        | DELETE             | unsubmitted                                                   |
| 19 | unsuported function                  | WordPress | /tags/{id}                                                              | DELETE             | unsubmitted                                                   |
| 20 | logical: duplicated id                        | WordPress | /users                                                                  | DELETE             | unsubmitted                                                   |


## Steps to reproduce bugs
We show the reproduction of some of the bugs, more detailed description and reproduction of the bugs can be viewed in the issue

**1. GET /users/{id}/custom_attributes** 

**GET/DELETE/PUT /users/{id}/custom_attributes/{key}**
1. create a user
```
Sending: POST server_host/api/v4/users?user_name=a
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}      
Received: 'HTTP/1.1 201 response:{"user_id":2}
```
2. delete the user
```
Sending: DELETE server_host/api/v4/users
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}      
Received: 'HTTP/1.1 202 response:{"message":"success"}
```
3. get the user's custom attributes
```
Sending: GET server_host/api/v4/users/{id}/custom_attributes 
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```


**2. GET /projects/{id}/custom_attributes**

**GET/DELETE/PUT /projects/{id}/custom_attributes/{key}**
similary with **GET /users/{id}/custom_attributes**



**3. GET /groups/{id}/custom_attributes**

**GET/DELETE/PUT /group/{id}/custom_attributes/{key}**
similary with **GET /users/{id}/custom_attributes**




**4.DELETE /projects/{id}/services/github**
1. create a project 
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. delete the project's "github" services
```
Sending: DELETE server_host/api/v4/projects/2/services/github
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```


**5. POST /hooks**
create a hook with invalid "url" (UTF-8)
```
Sending: POST server_host/api/v4/hooks  
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data: {"url": "%e5"} 
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**6. POST /projects/{id}/metrics/user_starred_dashboards**

1. create a project A
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. create a user starred dashboards with invalid "dashboard_path" (utf-8)
```
Sending: POST server_host/api/v4/projects/2/metrics/user_starred_dashboards
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {"dashboard_path": "%e6%99%ba%e8%83%bd%e5"}
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**7. POST /admin/cluster/add**

create a cluster with invalid "platform_kubernetes_attributes\[api_url\]"(UTF-8)

```
Sending: POST server_host/api/v4/admin/cluster/add
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {"platform_kubernetes_attributes": "%e5"}
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**8. POST /projects/{id}/cluster/user**
1. create a project
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. create a cluster for a project  with invalid "platform_kubernetes_attributes\[api_url\]"(UTF-8)
```
Sending: POST server_host/api/v4/projects/cluster/user
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {"platform_kubernetes_attributes": "%e5"}
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**9. POST /groups/{id}/cluster/user**
similary with  **8. POST /projects/{id}/cluster/user**

**10. POST /projects/{id}/export**
similary with  **8. POST /projects/{id}/cluster/user**

**11. GET /projects/{id}/variables/{key}**
1. create a project
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}    
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. get a project variables with a invalid "filter"(special characters)
```
Sending: GET server_host/api/v4/projects/2/variables/fuzzstring?fileter=1'
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**12. GET /projects/{id}/environments**
1. create a project
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. get a project environments with a invalid "states"(not enum)
```
Sending: GET server_host/api/v4/projects/2/environments?states=a
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```



**13. GET /projects/{id}/repository/commits**

1. create a project 
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. get the project commits with length of parameter 'ref_name' is too long and has special characters
```
Sending: GET server_host/api/v4/projects/2/repository/commits?ref_name=email:1@gmail.com
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**14. POST /projects/{id}/repository/commits**  
1、Create a new project  
```
Sending: POST server_host/api/v4/projects?name=a
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2、Create a commit for the new project with special characters ":" in the branch parameter:
```
Sending: POST server_host/api/v4/projects/{project_id}/repository/commits
header: {'Content-Type': 'application/json',
          'Authorization': 'Bearer token'}
data:{"branch": "email:",
      "commit_message": "suaxpicd7f",
      "actions": [{"action": "create",
                   "file_path": "8apwey0w5h", 
                   "execute_filemode": "False"}]}
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```


**15. POST /projects/{id}/repository/branches**

1. create a project with an invalid "import_url"  
```
Sending: POST server_host/api/v4/projects?name=a
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:{"import_url": "invalid import_url"}
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. post "main" branch in this project
```
Sending: POST server_host/api/v4/projects/{project_id}/repository/branches?branch=main&ref=main 
data:
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**16.POST /projects/{id}/fork/{forked_from_id}** 

1. create a project A 
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
2. create a project B 

```
Sending: POST server_host/api/v4/projects?name=B
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"project_id": 3}
```
3. project B fork project A
```
Sending: POST server_host/api/v4/projects/2/fork/3 
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 201 response : {"message":"success"} 
```
4. project A fork project B  
```
Sending: POST server_host/api/v4/projects/3/fork/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```


**17. POST /projects**
create a project with the optional parameter 'use_custom_template'

```
Sending: POST server_host/api/v4/projects?name=Administrator   
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data: {"use_custom_template": "False"}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```
**18. DELETE /categories/{id}** 
1. create a categories
```
Sending: POST server_host/wp-json/wp/v2/categories
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {'name': 'a'}
Received: 'HTTP/1.1 201 response : {"id": 2}
```
2. delete the categories
```
Sending: DELETE server_host/wp-json/wp/v2/tags/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 501 response : {"code":"rest_trash_not_supported"} 
```
**19. DELETE /tags/{id}**
1. create a tag
```
Sending: POST server_host/wp-json/wp/v2/tags
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {'name': 'a'}
Received: 'HTTP/1.1 201 response : {"id": 2}
```
2. delete the tag
```
Sending: DELETE server_host/wp-json/wp/v2/tags/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 501 response : {"code":"rest_trash_not_supported"} 
```

**20. POST /users** ------ use existed user email or user name
1. create a user A
```
Sending: POST /users server_host/wp-json/wp/v2/users 
API_id: 35 header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}
data: {"username": "A", "name": "jqn6eec4uz", "email": "5@BS.yoM", "password": "string", "description": "string"}
Received: 'HTTP/1.1 201 response : {"id":"1"}
```

2. create a user A again

```
Sending: POST /users server_host/wp-json/wp/v2/users 
API_id: 35 header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}
data: {"username": "A", "name": "jqn6eec4uz", "email": "5@BS.yoM", "password": "string", "description": "string"}
Received: 'HTTP/1.1 500 response : {"code":"existing_user_login"}
```
