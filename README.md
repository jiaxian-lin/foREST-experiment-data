# foREST-experiment-data

## experiment bugs found
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

### gitlab commits

#### foREST

**GET /projects/{id}/repository/commits**

1. create a project 
2. get the project commits with length of parameter 'ref_name' is too long

for example:
```
Sending: POST server_host/api/v4/projects?name=A
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
```
Sending: GET server_host/api/v4/projects/2/repository/commits?ref_name=longlongstring
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

## Bugs found in other APIs

this part list the Bugs we found by foREST , 

| Date | Project | Link | Status | Description |
|---------|---------|---------|---------|---------|
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) | submitted | POST  /hooks |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) | submitted | POST  /admin/clusters/add |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /clusters/{id}/metrics_dashboard/annotations/ |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | DELETE/PUT/GET  /users/{id}/custom_attributes/{key} |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | GET  /users/{id}/custom_attributes |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /projects/{id}/clusters/user |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) | submitted | POST  /projects/{id}/metrics/user_starred_dashboards |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | DELETE/POST  /projects/{id}/custom_attributes/{key} |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /projects/{id}/export | 
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | GET  /projects/{id}/custom_attributes |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | POST  /groups/{id}/clusters/user |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | GET /groups/{id}/custom_attributes |
| 2021-7-4 | GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | DELETE/PUT/GET  /groups/{id}/custom_attributes/{key} |
| 2021-7-4 | WordPress |  | unsubmitted | POST  /categories |
| 2021-11-4| Gitlab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346563) | submitted | POST projects/{id}/fork/{forked_from_id} |
