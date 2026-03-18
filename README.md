# elastic-beanstalk-python-arista
# AWS Elastic Beanstalk Python Sample Application

A modified Python sample application for AWS Elastic Beanstalk deployment, created as part of an AWS learning activity.

## Modified Features

- Updated welcome message to include developer name
- Python Flask application with custom templates
- Environment information display

## Prerequisites

- AWS Account with access to Elastic Beanstalk
- Existing VPC with public and private subnets (with NAT route)
- AWS CLI configured (optional, for CLI commands)
- Python 3.7+ installed locally

## Application Files

### Modified application.py

```python
from flask import Flask, render_template
import socket
import os
import datetime

application = Flask(__name__)

@application.route('/')
def index():
    hostname = socket.gethostname()
    environment = os.environ.get('ENVIRONMENT_NAME', 'development')
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    return render_template('index.html', 
                         hostname=hostname,
                         environment=environment,
                         timestamp=timestamp,
                         version='0.0.2')

@application.route('/health')
def health():
    return {
        'status': 'healthy',
        'timestamp': datetime.datetime.now().isoformat(),
        'version': '0.0.2'
    }

@application.route('/info')
def info():
    return {
        'application': 'Python Sample App',
        'version': '0.0.2',
        'environment': os.environ.get('ENVIRONMENT_NAME', 'unknown'),
        'platform': 'AWS Elastic Beanstalk',
        'python_version': os.sys.version
    }

if __name__ == '__main__':
    application.run(host='0.0.0.0', port=8080)
