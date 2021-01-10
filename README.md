# jupyter-continumm

# We acknowledge Mikhail Titov for his work on RADICAL-DREAMER.

## Requirements

* Python 3.6+ with the following libraries:
  * [radical.utils](https://github.com/radical-cybertools/radical.utils) 1.5.7+
  * numpy
  * pika
* RabbitMQ (**running** instance that is installed either locally or remotely)

## Installation
1) Virtual environment
```shell script
# python3 virtualenv
python3 -m venv ./dreamer
source ./dreamer/bin/activate
```
OR
```shell script
# conda virtualenv
conda create -n dreamer python=3.7 -y
conda activate dreamer
```
2) `radical.dreamer` package
```shell script
pip install git+https://github.com/radical-project/radical.dreamer.git
```
NOTE: For test purposes to have the latest (**unstable**) version please use 
the `devel` branch
```shell script
pip install git+https://github.com/radical-project/radical.dreamer.git@devel
```

## Executing of provided example
Export RabbitMQ URL at each terminal where executable runs
```shell script
export RADICAL_DREAMER_RMQ_URL="amqp://localhost:5672/"
```
NOTE (1): This env variable is not needed if the corresponding URL is set in 
the config using either the class `Config` or JSON file

NOTE (2): Provided RabbitMQ URL is a default value and it is for the local 
installation of RabbitMQ. If remotely installed RabbitMQ is used, then, please, 
set the URL of the following format (username/password are optional)
`"amqp://<username>:<password>@<host>:<port>/"` 
([official docs](https://www.rabbitmq.com/uri-spec.html))

Run ResourceManager (1st terminal)
```shell script
# firstly activate corresponding virtualenv
radical-dreamer-start-manager
```
Run the example of Session (2nd terminal), which sets Resource and Workload 
descriptions
```shell script
# firstly activate corresponding virtualenv
wget -q https://raw.githubusercontent.com/radical-project/radical.dreamer/master/examples/run_session.py
# (!) for devel branch:
# wget -q https://raw.githubusercontent.com/radical-project/radical.dreamer/devel/examples/run_session.py
chmod +x run_session.py
./run_session.py
```

## Config parameters
Configuration parameters could be set by using the corresponding class:
```python
from radical.dreamer import Config, Session

cfg_data = {
    'rabbitmq': {
        'url': 'amqp://localhost:5672/'
    },
    'session': {
        'profile_base_name': 'rd.profile'
    },
    'schedule': {
        'strategy': 'smallest_to_fastest',
        'early_binding': True
    }
}
session = Session(cfg=Config(cfg_data))
```
Another option is to have all that parameters in the dedicated JSON file (e.g., 
`examples/config_data.json`). And below, there are examples of using such JSON 
file as config.
```shell script
# run ResourceManager
radical-dreamer-start-manager --cfg_path examples/config_data.json
```
