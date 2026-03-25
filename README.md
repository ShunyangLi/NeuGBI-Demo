# Source code and docs of "ClawGraph: Bridging Massive Graph Data Lakes for Autonomous Agent Reasoning"

The demo scenario report is available here: ![report](report_en.pdf)

## NeuGBI Service Startup Guide

This document explains how to start the NeuGBI-related services (`graphcube-server` and `openclaw gateway`) within a Docker container.

## 1. Pull the Image

``` bash
docker pull shunyangli/neugbi:latest
```

## 2. Create and Start the Container (Port Mapping)

When starting the container, ensure that port `18891` on the host is mapped to port `18891` in the container:

``` bash
docker run -dit --name NeuGBI -p 18891:18891 shunyangli/neugbi:latest
```

Notes:

- `--name NeuGBI`: Assigns the name "NeuGBI" to the container for easier management and access.
- `-p 18891:18891`: Maps host port `18891` to container port `18891`, enabling external access to the gateway service.

## 3. Enter the Container

After the container is started, execute the following command to enter it:

```bash
docker exec -it NeuGBI /bin/bash
```

## 4. Start Services

### 4.1 Start the GraphCube Service

The `graphcube-server` must bind to port `8073`. It is recommended to run it in the background:

``` bash
nohup graphcube-server 8073 > /tmp/graphcube.log 2>&1 &
```

### 4.2 Configure Model and API Key (Execute First)

Before starting the gateway, navigate to the `openclaw` directory and manually complete the model and API Key configuration:

```bash
cd /neug/openclaw
pnpm openclaw onboard --install-daemon
```

After completion, you can verify the setup by accessing the following URL:

```text
http://localhost:18891/#token=token=1f4adb52935eb40448301f8a626b77f3bb01096010199846
```

### 4.3 Start the OpenClaw Gateway

Once the model and API Key are configured, start the gateway service. The port must match the Docker-mapped port `18891`:

```bash
cd /neug/openclaw
pnpm openclaw gateway --port 18891 --allow-unconfigured --bind lan
```

### 4.4 Data Import

After the services are started, input the following command in the OpenClaw dialog box to import data and view the schema:

```text
Please use the graphcube skill to load data from /neug/neug_db and display the complete data schema.
```

### 4.5 BI Analysis

Once data import is complete, input the following command in the OpenClaw dialog box to perform BI analysis and generate a report:

```text
Please use the bi analyze skill to analyze "the impact of the Russia-Ukraine conflict on the Russian academic field" and output a structured analysis report.
```

### 4.6 Load OpenAIRE Dataset

If you wish to use the OpenAIRE dataset, execute the following command:

```shell
cd /neug 
./load_openaire.sh
```

This script will download the NeuG OpenAIRE dataset from OSS. You can then load and query it using the commands described in sections **4.4** and **4.5**.
