<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# OpenWhisk

[![Build Status](https://travis-ci.com/apache/openwhisk.svg?branch=master)](https://travis-ci.com/apache/openwhisk)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Join Slack](https://img.shields.io/badge/join-slack-9B69A0.svg)](https://openwhisk-team.slack.com/)
[![codecov](https://codecov.io/gh/apache/openwhisk/branch/master/graph/badge.svg)](https://codecov.io/gh/apache/openwhisk)
[![Twitter](https://img.shields.io/twitter/follow/openwhisk.svg?style=social&logo=twitter)](https://twitter.com/intent/follow?screen_name=openwhisk)

OpenWhisk is a serverless functions platform for building cloud applications.
OpenWhisk offers a rich programming model for creating serverless APIs from functions,
composing functions into serverless workflows, and connecting events to functions using rules and triggers.
Learn more at [http://openwhisk.apache.org](http://openwhisk.apache.org).


### Quick Start

The easiest way to start using OpenWhisk is to install the "Standalone" OpenWhisk stack.
This is a full-featured OpenWhisk stack running as a Java process for convenience.
Serverless functions run within Docker containers. You will need [Docker](https://docs.docker.com/install),
[Java](https://java.com/en/download/help/download_options.xml) and [Node.js](https://nodejs.org) available on your machine.

To get started:
```
git clone https://github.com/apache/openwhisk.git
cd openwhisk
./gradlew core:standalone:bootRun
```

- When the OpenWhisk stack is up, it will open your browser to a functions [Playground](./docs/images/playground-ui.png),
typically served from http://localhost:3232. The Playground allows you create and run functions directly from your browser.

- To make use of all OpenWhisk features, you will need the OpenWhisk command line tool called
`wsk` which you can download from https://s.apache.org/openwhisk-cli-download.
Please refer to the [CLI configuration](./docs/cli.md) for additional details. Typically you
configure the CLI for Standalone OpenWhisk as follows:
```
wsk property set \
  --apihost 'http://localhost:3233' \
  --auth '23bc46b1-71f6-4ed5-8c54-816aa4f8c502:123zO3xZCLrMN6v2BKK1dXYFpXlPkccOFqm12CdAsMgRU4VrNZ9lyGVCGuMDGIwP'
```

- Standalone OpenWhisk can be configured to deploy additional capabilities when that is desirable.
Additional resources are available [here](./core/standalone/README.md).

### Edit the system

Guidance to install the OpenWhisk CLI: https://github.com/apache/openwhisk#quick-start

- The keep-alive policy of the system is located in the folder 
[./core/invoker/src/main/scala/org/apache/openwhisk/core/containerpool/](./core/invoker/src/main/scala/org/apache/openwhisk/core/containerpool/). 

- All the designs in python language are located in the file
[./pythonlogics/LambdaScheduler.py](./pythonlogics/LambdaScheduler.py)

- To run the test, you can edit the data and settings in [./pythonlogics/many_run.py](./pythonlogics/many_run.py) and then run the file.

To set up the system, you can edit the [./sample-app.conf](./sample-app.conf) in the root and put it in [./bin/](./bin/).
Rename it to [./bin/application.conf](./bin/application.conf). The system requires this configuration file to run and allow function creation. You can adjust container-pool.user-memory depending on local resources.

- [./run.sh](./run.sh) will build and run the custom OpenWhisk. 

The sample test is in the folder [./wsk-actions/](./wsk-actions/)

- Build the docker file located at [./wsk-actions/py/Dockerfile](./wsk-actions/py/Dockerfile) 
With the tag alfuerst/wsk-py-pybuild. Which can be changed in the file
[./wsk-actions/py/build.sh](./wsk-actions/py/build.sh) 
This will create zip packages for all the actions that the system can use. 

- To run the load tests, it will also have to edit the Docker container in 
[./wsk-actions/load-test/lookbusy/](./wsk-actions/load-test/lookbusy/).
The new Docker container should be added as a runtime in ./ansible/files/runtimes.json.

- [./wsk-actions/load-test/wsk_interact.py](./wsk-actions/load-test/wsk_interact.py) contains helper functions that set up functions and authentication.
