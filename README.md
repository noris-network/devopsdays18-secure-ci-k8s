# Workshop: Secure Continuous Delivery in Kubernetes Clusters 

This repository contains the files that were used during the
demo session `Workshop: Secure Continuous Delivery in Kubernetes Clusters`
at DevOpsDays Berlin 2018.

## Usage

The manifests `gocd-server.yml` and `gocd-agent.yml` can be directly
used in your Kubernetes cluter via the `create` command:

```shell
kubectl create -f gocd-server.yml
kubectl create -f gocd-agent.yml
```

To be able, to deploy the dedicated GoCD build agents, one needs to
build the Dockerfile in `gocd-build-agent/`:

```
docker build -t local/gocd-build-agent:v1 .
```

If you do not have an own private registry or Docker hub account,
you need to build this image on every dedicated build host, so that
the image is available on every host that needs it.

Then, one can create the deployment:
```shell
kubectl create -f gocd-build-agent.yml
```

## Exemplary GoCD pipeline

Remember to assign the resource `buildhost` to every `gocd-build-agent` from within the GoCD agent tab.

The following pipeline illustrates the usage:
```xml
<pipelines group="DevOpsDays">
  <pipeline name="DemoPipeline">
    <materials>
      <git url="YOUR_URL_HERE" />
    </materials>
    <stage name="DemoStage">
      <jobs>
        <job name="DemoJob">
          <tasks>
            <exec command="sudo">
              <arg>dockerw</arg>
              <arg>version</arg>
            </exec>
            <exec command="sudo">
              <arg>dockerw</arg>
              <arg>build</arg>
              <arg>-t</arg>
              <arg>10.32.192.115:5000/hello-world</arg>
              <arg>.</arg>
              <runif status="passed" />
            </exec>
            <exec command="sudo">
              <arg>dockerw</arg>
              <arg>push</arg>
              <arg>10.32.192.115:5000/hello-world</arg>
              <runif status="passed" />
            </exec>
          </tasks>
          <resources>
            <resource>buildhost</resource>
          </resources>
        </job>
      </jobs>
    </stage>
  </pipeline>
</pipelines>
```

Replace `YOUR_URL_HERE` with a URL to a repository that contains a Dockerifle.

## Legal information
For legal information, see `LICENSE.md`.

## Authors
Oz Tiram <oz.tiram@noris.de>
Jonas Röckl <jonas.roeckl@noris.de>
