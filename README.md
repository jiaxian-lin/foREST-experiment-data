# foREST-experiment-data

## all bugs found
### gitlab group
#### foREST

POST /groups/{id}/hooks 

DELETE /groups/{id}

#### EvoMaster

GET /api/v4/groups/{id}/subgroups     parameter 'all_avail' and 'ablestatistics' Simultaneously

#### RESTler

No Bug found

### gitlab project

#### foREST

POST /projects                  with the parameter 'use_custom_template'

POST /projects/{id}/fork/{forked_from_id}  Circular fork(include fork itself)

POST /projects/{id}/share      

#### EvoMaster

No Bug found

#### RESTler

POST /projects                        with the parameter 'use_custom_template'

POST /api/v4/projects/user/1           with the parameter 'use_custom_template'

### gitlab commits

#### foREST

GET /projects/{id}/repository/commits    length of parameter 'ref_name' is too long

POST /projects/{id}/repository/commits

POST /projects/{id}/repository/branches

#### EvoMaster

No BUG found

#### RESTler

No BUG found

### WordPress

#### foREST

DELETE /tags/{id}         rest trash not supported

POST /users               use existed user email

DELETE /categories/{id    rest trash not supported

#### EvoMaster

No BUG found

#### RESTler

No BUG found

## Bugs found in other APIs


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
