

# foREST



**foREST** is a stateful REST API fuzzing tool based on OpenAPI documentation for automatically testing cloud services through REST APIs and finding security and reliability bugs in those services. foREST analyzes the OpenAPI document and then generates and executes test cases to test the cloud service.

foREST will automatically infer the producer-consumer relationship between cloud service APIs based on the OpenAPI document, build a dependency tree, and generate test cases that satisfy the dependencies based on the dependency tree. This method enables foREST to intelligently generate test cases , improve the efficiency of test case generation, and find more bugs.


## how to run 



1. Store the yaml document of the service to be tested in the openapi folder

2. Configure in the [FoREST_config](https://github.com/jiaxian-lin/foREST-experiment-data/blob/main/code/foREST/FoREST_config.conf) according to actual needs

3. Install the dependencies required to run foREST
```bash
pip3 install -r requirements.txt
```
4. run
```bash
python3 main.py
```

