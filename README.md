# foREST-experiment-data
This is the companion repo for the paper **foREST: A Tree-based Approach for Fuzzing RESTful APIs** submitted to ISSTA 2022.

This repository is divided in two folders:
1. `source code`:contains the tools described in the paper
2. `experiment data`:contains the experiments data and how to reproduce the experiment

A summary of the contents of this page is as follows:
1. [RESTler random-walk mode experiment](https://github.com/jiaxian-lin/foREST-experiment-data#restler-randomwalk-mode): This part shows the actual situation of RESTler's random walk mode
2. [Summary of Bugs found by foREST](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#bugs-found-in-experiment-by-forest): This part shows the summary of bugs found by foREST
3. [Bugs found in other APIs](https://github.com/jiaxian-lin/foREST-experiment-data#bugs-found-in-other-apis): This part shows the bugs found by foREST during testing
4. [Experiment bugs found](https://github.com/jiaxian-lin/foREST-experiment-data#experiment-bugs-found): This part shows the bugs found by each tool during the experiment
## RESTler randomwalk mode experiment
<img src="https://user-images.githubusercontent.com/71680354/160048141-4fb2b6af-d44d-4ff0-b6c7-c597d41778c0.png" width = "500" height = "400" align=center />
<img src="https://user-images.githubusercontent.com/71680354/160048216-5b284ba1-e2f8-4dec-b7da-dd1c9a5db918.png" width = "500" height = "400" align=center />

## Bugs found in experiment by foREST
| Project | Link | Description|
| --------- | ---------| -------- |
| GitLab-project | [detail](https://github.com/jiaxian-lin/foREST-experiment-data#forest-1) | POST /projects|
| GitLab-project | [detail](https://github.com/jiaxian-lin/foREST-experiment-data#forest-1) | POST /projects/{id}/fork/{forked_from_id}|
| GitLab-project | [detail](https://github.com/jiaxian-lin/foREST-experiment-data#forest-1)| POST /projects/{id}/share |
| GitLab-group | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#gitlab-group) | POST /groups/{id}/hooks |
| GitLab-group | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#gitlab-group) | DELETE /groups/{id} |
| GitLab-commits | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#gitlab-commits) | GET /projects/{id}/repository/commits |
| GitLab-commits | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#gitlab-commits) | POST /projects/{id}/repository/commits|
| GitLab-commits | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#gitlab-commits) | POST /projects/{id}/repository/branches|
| WordPress | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#wordpress) | DELETE /tags/{id} |
| WordPress | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#wordpress) | POST /users |
| WordPress | [detail](https://github.com/jiaxian-lin/foREST-experiment-data/edit/main/README.md#wordpress) | DELETE /categories/{id} |
## Bugs found in other APIs by foREST

this part list the Bugs we found by foREST 

| Project | Link | Status | Description |
|---------|---------|---------|---------|
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) | submitted | POST  /hooks |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) | submitted | POST  /admin/clusters/add |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /clusters/{id}/metrics_dashboard/annotations/ |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | DELETE/PUT/GET  /users/{id}/custom_attributes/{key} |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | GET  /users/{id}/custom_attributes |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /projects/{id}/clusters/user |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) | submitted | POST  /projects/{id}/metrics/user_starred_dashboards |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | DELETE/POST  /projects/{id}/custom_attributes/{key} |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /projects/{id}/export | 
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | GET  /projects/{id}/custom_attributes |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /groups/{id}/clusters/user |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | GET /groups/{id}/custom_attributes |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | DELETE/PUT/GET  /groups/{id}/custom_attributes/{key} |
| WordPress |  | unsubmitted | POST  /categories |
| Gitlab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346563) | submitted | POST projects/{id}/fork/{forked_from_id} |

## experiment bugs found
### gitlab project

#### foREST

**POST /projects.** 

1. create a project with the optional parameter 'use_custom_template'

for example:
```
Sending: POST server_host/api/v4/projects?name=Administrator   
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data: {"use_custom_template": "False"}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```
**POST /projects/{id}/fork/{forked_from_id}** 

1. create a project A 
2. create a project B 
3. project B fork project A
4. project A fork project B  

for example:
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
```
Sending: POST server_host/api/v4/projects?name=B
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"project_id": 3}
```
```
Sending: POST server_host/api/v4/projects/2/fork/3 
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"message":"success"} 
```
```
Sending: POST server_host/api/v4/projects/3/fork/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**POST /projects/{id}/share**      

#### EvoMaster

No Bug found

#### RESTler

**POST /projects** 

1. create a project with the optional parameter 'use_custom_template'

for example:
```
Sending: POST server_host/api/v4/projects?name=Administrator   
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data: {"use_custom_template": "False"}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**POST /api/v4/projects/user/id**

1. create a project for special user with the optional parameter 'use_custom_template'

for example:
```
Sending: POST server_host/api/v4/projects/user?name=Administrator   
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data: {"use_custom_template": "False"}  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```
### gitlab group
#### foREST

**POST /groups/{id}/hooks**    
**DELETE /groups/{id}**

#### EvoMaster

**GET /api/v4/groups/{id}/subgroups**

1. create a groups A
2. get A's subgroups with parameter 'all_avail' and 'statistics'

for example:

```
Sending: POST server_host/api/v4/group?name=a&path=b
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"group_id": 2}
```

```
Sending: GET server_host/api/v4/group/2/subgroups?all_available=false&statistics=true
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```


#### RESTler

No Bug found


### gitlab commits

#### foREST

**GET /projects/{id}/repository/commits**

1. create a project 
2. get the project commits with length of parameter 'ref_name' is too long and has special characters ':'

for example:
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
```
Sending: GET server_host/api/v4/projects/2/repository/commits?ref_name=email:1@gmail.com
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```

**POST /projects/{id}/repository/commits**  
**POST /projects/{id}/repository/branches**

#### EvoMaster

No BUG found

#### RESTler

No BUG found

### WordPress

the Bugs we found in WordPress caught by the server and returned the error reason

#### foREST

**DELETE /tags/{id}**
1. create a tag
2. delete the tag

for example:
```
Sending: POST server_host/wp-json/wp/v2/tags
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {'name': 'a'}
Received: 'HTTP/1.1 201 response : {"id": 2}
```
```
Sending: DELETE server_host/wp-json/wp/v2/tags/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 501 response : {"code":"rest_trash_not_supported"} 
```

**POST /users** ------ use existed user email  
1. create a user A
2. create a user A again

```
Sending: POST /users server_host/wp-json/wp/v2/users 
API_id: 35 header:{'Content-Type': 'application/json', 'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC8xOTIuMTY4LjExMi4xOTQiLCJpYXQiOjE2NDI1NjQ2NTQsIm5iZiI6MTY0MjU2NDY1NCwiZXhwIjoxNjQzMTY5NDU0LCJkYXRhIjp7InVzZXIiOnsiaWQiOiIxIn19fQ.MSGSpG7__uMcW_TQwMOrsgoNvUX4ouOqLIARBUoT3to'}
data: {"username": "A", "name": "jqn6eec4uz", "email": "5@BS.yoM", "password": "string", "description": "string"}
Received: 'HTTP/1.1 201 response : {"id":"1"}
```
```
Sending: POST /users server_host/wp-json/wp/v2/users 
API_id: 35 header:{'Content-Type': 'application/json', 'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC8xOTIuMTY4LjExMi4xOTQiLCJpYXQiOjE2NDI1NjQ2NTQsIm5iZiI6MTY0MjU2NDY1NCwiZXhwIjoxNjQzMTY5NDU0LCJkYXRhIjp7InVzZXIiOnsiaWQiOiIxIn19fQ.MSGSpG7__uMcW_TQwMOrsgoNvUX4ouOqLIARBUoT3to'}
data: {"username": "A", "name": "jqn6eec4uz", "email": "5@BS.yoM", "password": "string", "description": "string"}
Received: 'HTTP/1.1 500 response : {"code":"existing_user_login"}
```

**DELETE /categories/{id}** ------ use the correct id rest trash not supported
1. create a categories
2. delete the categories

for example:
```
Sending: POST server_host/wp-json/wp/v2/categories
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  {'name': 'a'}
Received: 'HTTP/1.1 201 response : {"id": 2}
```
```
Sending: DELETE server_host/wp-json/wp/v2/tags/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 501 response : {"code":"rest_trash_not_supported"} 
```
#### EvoMaster

No BUG found

#### RESTler

No BUG found


