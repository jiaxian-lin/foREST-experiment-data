# foREST-experiment-data

## experiment bugs found
### gitlab group
#### foREST

POST /groups/{id}/hooks  
DELETE /groups/{id}

#### EvoMaster

GET /api/v4/groups/{id}/subgroups ------ parameter 'all_avail' and 'able_statistics' Simultaneously

#### RESTler

No Bug found

### gitlab project

#### foREST

**POST /projects.** send a request with the optional parameter 'use_custom_template',for example:  
```
Sending: POST server_host/api/v4/projects?name=Administrator   
API_id: 0 header:{'Content-Type': 'application/json', 'Authorization': 'Bearer mbz3FKZTFfLoyMBm515E'}  
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
Sending: POST server_host/api/v4/projects/2/fork/1 
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 201 response : {"message":"success"} 
```
```
Sending: POST server_host/api/v4/projects/1/fork/2
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:  
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```
POST /projects/{id}/share      

#### EvoMaster

No Bug found

#### RESTler

POST /projects ------ with the parameter 'use_custom_template'  
POST /api/v4/projects/user/1 ------ with the parameter 'use_custom_template'

### gitlab commits

#### foREST

GET /projects/{id}/repository/commits ------ length of parameter 'ref_name' is too long  
POST /projects/{id}/repository/commits  
POST /projects/{id}/repository/branches

#### EvoMaster

No BUG found

#### RESTler

No BUG found

### WordPress

the Bugs we found in WordPress caught by the server and returned the error reason

#### foREST

DELETE /tags/{id} ------ use the correct id and the rest trash not supported  
POST /users ------ use existed user email  
DELETE /categories/{id} ------ use the correct id rest trash not supported

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
