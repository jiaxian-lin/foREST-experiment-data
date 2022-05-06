# Experiment reproduce
## introduction
The experimental reference tools are [RESTler](https://github.com/microsoft/restler-fuzzer) and [EvoMaster black-box-mode](https://github.com/EMResearch/EvoMaster/blob/master/docs/blackbox.md) 
To make the experiment more general, we do not presuppose values other than the necessary conditions 
The commands used in the experiment are as follows：
### restler:
1. build

```
python ./build-restler.py --dest_dir <full path to restler_bin above>
```
2. compile
```
./Restler.exe compile --api_spec 
```
3. fuzz
```
./Restler.exe fuzz --grammar_file ./Compile/grammar.py --dictionary_file ./Compile//dict.json --token_refresh_interval 360000 --token_refresh_command 'python3 ../main.py' --no_ssl --time_budget 6 --settings ..\setting.json
```
4. setting.json
```
{
    "max_combinations": 20,
    "max_request_execution_time": 90,
    "max_async_resource_creation_time": 60,
    "global_producer_timing_delay": 2,
    "dyn_objects_cache_size":20,
    "fuzzing_mode": "random-walk",
    "test_combinations_settings": {
       "query_param_combinations": {
        "max_combinations": 50,
        "param_kind": "required"
      },
       "header_param_combinations": {
        "max_combinations": 50,
        "param_kind": "required"
      }
    },
    "checkers": {
        "useafterfree" : {
            "mode" : "exhaustive"
        },
        "leakagerule" : {
            "mode" : "normal"
        },
        "resourcehierarchy" : {
            "mode" : "exhaustive"
        }
    }
}
```

### EvoMaster
We fuzz by using `eovmaster.jar` provided by EvoMaster
```
java -jar evomaster.jar --blackBox true --bbSwaggerUrl file:///<open api file path> --outputFormat JAVA_JUNIT_4 --maxTime 21600s --bbTargetUrl <SUT_url> --header0 'Authorization:Bearer <token>' --header1 'a:a' --header2 'b:b'
```
## GitLab
The minimum requirement for GitLab to run is 4 cores and 8G, so make sure your machine can meet the conditions

The GitLab coverage tool is integrated with GitLab in docker, and is used in the following way(in ubuntu):

### install docker

```
curl -fsSL https://get.docker.com | bash -s docker
```

### Verify installation
```
docker -v
```

### Create a folder to store all data, logs, etc.
```
mkdir gitlab
export GITLAB_HOME={path to dir}/gitlab
cd {path to dir}/gitlab
```

### install GitLab
```
sudo docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 23:23 \
  --name gitlab-api \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  witcan/gitlab-ee-api
```
Takes about a minute to start up after installation

Visit http://localhost to access the main GitLab interface and confirm the GitLab running status, as shown in the figure

### Change the password of the root user account

1. Get the GitLab container id
```
docker ps
```
2. Enter the container
```
docker exec -it gitlab-api bash
```
3. change the password

```
gitlab-rails console
user = User.where(id:1).first
user.password = 'password'
user.password_confirmation = 'password'
user.save!
quit
```

### How to get and reset code coverage?
1. get coverage
```
GET http://localhost/api/v4/templates/get_coverage
```
2. reset covearge
```
POST http://localhost/api/v4/templates/reset_coverage
```




