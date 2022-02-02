# KustomizeGoat - Vulnerable by design Kustomize deployment
[![Maintained by Bridgecrew.io](https://img.shields.io/badge/maintained%20by-bridgecrew.io-blueviolet)](https://bridgecrew.io/?utm_source=github&utm_medium=organic_oss&utm_campaign=kustomizegoat)

![Terragoat](images/kustomizegoat-logo.png)

Demonstrating secure and non secure kubernetes IaC manifests using Kustomize.io (`kubectl -k`) overlays.

## Whats in the repo

The manifests are based on the [following blog post](https://bridgecrew.io/blog/creating-a-secure-kubernetes-nginx-deployment-using-checkov/), which demonstrates howto take a basic NGINX kubernetes deployment with many security issues, and use checkov to produce a fully compliant manifest to acheive the same NGINX deployment.

Using kustomize overlays (environments) we see both forms of these configurations here:

* `kustomize/base` - Our base manifests, similar to the *starting* manifests in the blog post, insecure.

* `kustomize/overlays/test` - A few security updates, but still a lot of non compliance.

* `kustomize/overlays/dev` -  An example of an empty overlay, produces the same results as `base` when merged with `kustomize build`

* `kustomize/overlays/prod` - Fully compliant additions to `base`, this overlay renders a clean bill of health when scanned with [Checkov.io's new Kustomize support!](https://www.checkov.io/7.Scan%20Examples/Kustomize.html)

## Scanning with Checkov.io

Simply clone this repository, and point `checkov` at the git checkout path, Checkov's Kustomize framework will traverse the directories, find bases and overlays and template them out, finally running all of the builtin Kubernetes security policies against each of the rendered templates.

```
checkov --framework kustomize -d ./kustomizegoat
```

![Checkov Kustomize Output](/images/checkov-kustomize-1.png)

Checkov will provide results for each base and each overlay seperately, allowing you to see misconfigurations specific to each environment and wether those security issues are inherited from your base manifests.

To see this more clearly, we can ask Checkov to just return a single policy, such as `CKV_K8S_11: CPU limits should be set` from the CIS Kubernetes guidelines.

Here we can clearly see only the `prod` overlay passes, with all over overlays (and the base manifests) failing the policy.


![Checkov Kustomize Output](/images/checkov-kustomize-2.png)

We also added the `--compact` flag to reduce CLI output for the screenshots, otherwise the specific templated manifest would also be shown with the failed policies, like so:

![Checkov Kustomize Output](/images/checkov-kustomize-3.png)

### Contributing

PR's and suggestions for further examples which highlight Kubernetes security posture are always welcome! 



## Bridgecrew's IaC herd of goats

* [CfnGoat](https://github.com/bridgecrewio/cfngoat) - Vulnerable by design Cloudformation template
* [TerraGoat](https://github.com/bridgecrewio/terragoat) - Vulnerable by design Terraform stack
* [CDKGoat](https://github.com/bridgecrewio/cdkgoat) - Vulnerable by design CDK application
* [KustomizeGoat](https://github.com/bridgecrewio/kustomizegoat) - Vulnerable by design kustomize deployment