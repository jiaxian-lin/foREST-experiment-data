# foREST-experiment-data
This is the companion repo for the paper **foREST: A Tree-based Approach for Fuzzing RESTful APIs** submitted to ASE 2022.

This repository is divided in two folders:
1. `source code`:contains the tools described in the paper
2. `experiment data`:contains the experiments data and how to reproduce the experiment

A summary of the contents of this page is as follows:
1. Summary of Bugs found by foREST: This part shows the summary of bugs found by foREST
2. Bugs found in other APIs: This part shows the bugs found by foREST during testing
3. Experiment bugs found: This part shows the bugs found by each tool during the experiment


## Some reported bugs found by foREST

this part list the Bugs we found by foREST 

| Project | Link | Status | Description |
|---------|---------|---------|---------|
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) | submitted | /hooks                                        POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346121) | submitted | /admin/clusters/add                           POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | /users/{id}/custom_attributes/{key} DELETE/PUT/GET |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | /users/{id}/custom_attributes                  GET |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360662) |  confirm  | /projects/{project_id}/variables/{key}         GET |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360138) |  confirm  | /projects/{id}/environments                    GET | 
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360147) | submitted | /projects/{id}/services/github               DELETE|
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | /projects/{id}/clusters/user                  POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334606) | submitted | /projects/{id}/metrics/user_starred_dashboards POST|
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | /projects/{id}/custom_attributes               GET |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | /projects/{id}/custom_attributes/{key} GET/DELETE/POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | /projects/{id}/export                         POST | 
| Gitlab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/346563) | submitted | /projects/{id}/fork/{forked_from_id}          POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360312) | submitted | /projects/{id}/repository/commits             POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/356922) | submitted | /projects/{id}/repository/commits              GET |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/360313) | submitted | /projects/{id}/repository/branches            POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/334610) | submitted | /groups/{id}/clusters/user                    POST |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | /groups/{id}/custom_attributes                 GET |
| GitLab | [issue](https://gitlab.com/gitlab-org/gitlab/-/issues/335276) | submitted | /groups/{id}/custom_attributes/{key} DELETE/PUT/GET |

## What are the characteristics of bugs found by foREST?
We roughly divide the bugs we find into three categories

-  better coverage
  1. DELETE/PUT/GET /users/{id}/custom_attributes/{key}
  2. GET /users/{id}/custom_attributes
  3. GET /projects/{id}/custom_attributes
  4. GET/DELETE/POST /projects/{id}/custom_attributes/{key}
  5. DELETE /projects/{id}/services/github 
  6. POST /projects/{id}/metrics/user_starred_dashboards
  7. GET /groups/{id}/custom_attributes
  8. DELETE/PUT/GET /groups/{id}/custom_attributes/{key}
-  fuzzing matching
  1. POST /hooks. (UTF-8)
  2. POST /admin/clusters/add. (UTF-8)
  3. POST /projects/{id}/clusters/user. (UTF-8)
  4. POST /groups/{id}/clusters/user. (UTF-8)
  5. POST /projects/{project_id}/variables/{key}. {invalid parameter}
  6. GET /projects/{id}/environments. (invalid parameter)
  7. POST /projects/{id}/metrics/user_starred_dashboards. (UTF-8)
  8. GET /projects/{id}/repository/commits. (invalid parameter)
  9. POST /projects/{id}/repository/commits. (invalid parameter)
  10. POST /projects/{id}/repository/branches. (invalid parameter)
-  resource pool
  1. POST /projects/{id}/fork/{forked_from_id}
## Steps to reproduce some bugs
We show the reproduction of some of the bugs, more detailed description and reproduction of the bugs can be viewed in the issue

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

### gitlab commits


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
1、Create a new project  
2、Create a commit for the new project with special characters ":" in the branch parameter:

```
Sending: POST server_host/api/v4/projects?name=a
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
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


**POST /projects/{id}/repository/branches**

1. create a project with an invalid "import_url"  
2. post "main" branch in this project

for example
```
Sending: POST server_host/api/v4/projects?name=a
header:{'Content-Type': 'application/json', 'Authorization': 'Bearer token'}  
data:{"import_url": "invalid import_url"}
Received: 'HTTP/1.1 201 response : {"project_id": 2}
```
```
Sending: POST server_host/api/v4/projects/{project_id}/repository/branches?branch=main&ref=main 
data:
Received: 'HTTP/1.1 500 response : {"message":"500 Internal Server Error"} 
```


### WordPress

the Bugs we found in WordPress caught by the server and returned the error reason


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

