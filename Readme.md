# Datadog Practical Take-Home Test

## Section 1: Practical Configuration Tasks

### Q1. Datadog Agent Installation

- **Task:** Install the Datadog Agent on a Linux machine (e.g., Ubuntu or CentOS).

- **Commands used:**
```bash
  # Update the apt package index
  sudo apt-get update
  
  # Install required dependencies
  sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
  
  # Add the Datadog repository
  DD_REPO="https://apt.datadoghq.com"
  echo "deb $DD_REPO stable 7" | sudo tee -a /etc/apt/sources.list.d/datadog.list
  
  # Import the Datadog GPG key
  curl -L https://keys.datadoghq.com/DATADOG_APT_KEY.public | sudo apt-key add -
  
  # Install the Datadog Agent
  sudo apt-get update && sudo apt-get install -y datadog-agent

  sudo nano /etc/datadog-agent/datadog.yaml [api_key: <YOUR_API_KEY>]

  # Start the Datadog Agent
  sudo systemctl start datadog-agent

```

- **Verification Command:**

```bash
  datadog-agent status
```

<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image2.png"/>
<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image1.png"/>

<hr>

### Q2. Host Monitoring with Custom Monitor

Task: Create a Datadog monitor that alerts if CPU usage exceeds 90% for more than 5 minutes. The alert should include tags and notify a dummy email address or Slack webhook.

Required - Monitor Configuration :
- CPU usage exceeds 90% for more than 5 minutes 
- The alert should include tags and notify a dummy email address or Slack webhook 

<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image3.png"/>
<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image4.png"/>
<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image5.png"/>
<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image6.png"/>
<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image7.png"/>

<hr>

### Q3. APM Setup for a Sample Application

Task: Set up APM for a basic application (Node.js, Python, or Java).

Installation and Configuration:

UsingNode.js:

```
sudo apt install npm
npm install --save dd-trace

// Example Node.js code to enable APM tracing
require('dd-trace').init({
  service: 'my-nodejs-app',
  env: 'dev',
  logInjection: true,
  version: '1.0.0',
  debug: true
});

const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Datadog APM!');
});

const port = 3000;
app.listen(port, () => {
  console.log(`App is listening on http://localhost:${port}`);
});
```


<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image8.png"/>

<hr>

### Q4. Basic APM Usage

Task: Generate some traffic or test calls and show a sample trace view.

Sample Trace Attributes:

- Trace ID: 12345

- Latency: 200ms

- Error: false

Explanation of the trace data:
This trace data helps identify performance bottlenecks, such as high latency in service calls. In this case, the latency of 200ms may indicate a delay in the request-response cycle which could be due to network or application-related issues.

<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image9.png"/>

<hr>

### Q5. Dashboard Creation

Task: Create a dashboard with the following widgets:

- **CPU usage (avg over time)**

- **Free disk space (%)**

- **APM latency or error rate for your app**

#### Widget Used and Queries:
- Time Series Chart - CPU Usage Metrics
- Table Chart - APM Metrics
- Bar Chart - Disk Usage

#### Queries:
- **CPU usage (avg over time):** avg(last_5m):avg:system.cpu.idle{host:your_host}
- **Free disk space (%):** avg(last_5m):avg:system.disk.free{host:your_host}
- **APM Latency/Error Rate:** avg(last_5m):avg:your_app.service.latency{env:dev}

Screenshot of the dashboard:

<img src="https://github.com/GitanshKapoor/TakeHomeTest/blob/main/Image10.png"/>

## Section 2: Troubleshooting Scenarios
### Q6. Agent Not Reporting Metrics

**Task:** You installed the Datadog Agent, but the host is not visible in Datadog. List at least 3 troubleshooting steps, including commands or logs you would check.

**Steps:**

```
Check Agent Status:
datadog-agent status
```

Helps confirm whether the agent is running or encountering errors.

```
Check Agent Logs:

tail -f /var/log/datadog/agent.log
```

Helps identify issues such as configuration errors or network problems.

```
Verify API Key Configuration:
cat /etc/datadog-agent/datadog.yaml | grep api_key
```

Ensures that the correct API key is configured to send data to the Datadog account.
<hr>

### Q7. APM Data Missing

Task: Your app has the APM library installed, but no data appears in Datadog APM. What could be the possible reasons?

**Possible Causes:**

**Missing or Incorrect Instrumentation:** Ensure that the APM library is correctly initialized in the application code.

**Incorrect Service Name Configuration:** Verify that the service name in the configuration matches the one in Datadog APM.

**Network Connectivity Issues:** Ensure that the application can reach the Datadog APM endpoints.

<hr>

### Q8. Monitor Didn’t Trigger Alert

Task: A monitor was set up correctly, but no alert was received even when the condition was met. List at least 3 common causes and how you would validate or fix them.

**Possible Causes:**

**Alert Condition Not Triggered:** Ensure the metric used in the alert query is correct and data points are being collected.

**No Notification Settings:** Verify that email or Slack notification settings are configured properly.

**Monitor Mute::** Check if the monitor was muted for maintenance or during a specific time window.
