# OpenFeature Observability Clinic

This repo is the companion to [this video](https://www.youtube.com/watch?v=efP2AqZ4BMg) and [this blog post](https://example.com/TODO).

## Preparation

You will need:

- A Linux VM with `docker` and `docker compose` installed
- A Dynatrace environment (https://dynatrace.com/trial)

# Gather Dynatrace Details
1. Make a note of your Dynatrace environment URL (e.g. `https://abc12345.live.dynatrace` without the trailing slash)
2. Create a new Access Token with the following permissions: `settings.write`, `Create and read synthetic monitors, locations, and nodes`

# Clone Repository
Clone this repository to the Linux VM:

```
cd ~
git clone https://github.com/agardnerIT/openfeature-perfclinic1
cd ~/openfeature-perfclinic1
```

# Set DT Environment Details

Modify the following with your details and run:
```
DT_ENVIRONMENT=https://abc12345.live.dynatrace.com
DT_TOKEN=dtc01.*****.*****
```

# Monitor Span Attributes
The OpenFeature demo contains 3 important span attributes.

Tell Dynatrace to automatically capture and store the values.

```
chmod +x ~/openfeature-perfclinic1/scripts/trackSpanAttributes.sh
~/openfeature-perfclinic1/scripts/trackSpanAttributes.sh $DT_ENVIRONMENT $DT_TOKEN
```

# Install OneAgent
Install a FullStack OneAgent. Go to your environment and click on "Deploy Dynatrace" > "Start Installation" > "Linux".

Best practice: Always set a host group. For example: `openfeature-demo`

Your installation command should look similar to this:
```
sudo /bin/sh Dynatrace-OneAgent-Linux-1.***.***.sh --set-infra-only=false --set-app-log-content-access=true --set-host-group=openfeature-demo
```

# Clone and Start OpenFeature Demo
```
cd ~
git clone https://github.com/open-feature/playground
cd playground
docker compose up --detach
```

The OpenFeature demo should now be accessible via a browser on port `30000` of your Linux VM.

# Apply Dynatrace Configuration

Finally, apply the Dynatrace configuration using [Monaco](https://dynatrace-oss.github.io/dynatrace-monitoring-as-code/).

First, change directory to the `monaco` folder and download [the latest Monaco binary](https://github.com/dynatrace-oss/dynatrace-monitoring-as-code/releases) to that directory:

```
cd ~/openfeature-perfclinic1/monaco
wget -O monaco https://github.com/dynatrace-oss/dynatrace-monitoring-as-code/releases/download/v1.8.7/monaco-linux-amd64
chmod +x monaco
export NEW_CLI=1
./monaco deploy -e environments.yaml
```

