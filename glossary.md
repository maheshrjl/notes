# 📙 Glossary

### Containerization

* **Immutable**: Immutable means that a container won't be modified during its life: no updates, no patches, no configuration changes.
* **Ephemeral**: [Temporary](https://dictionary.cambridge.org/dictionary/english/ephemeral) i.e. The container can be stopped and destroyed, then rebuilt and replaced with an absolute minimum set up and configuration very quickly.
* [**Immutable**](https://blog.maheshrjl.com/iacterms): Containers are not changed once they are running, they are re-deployed for any config change
* **Seperation of Concerns**: Contianer shouldn't contain unique data mixed in with application bianaries
* **Persistence**: Data present on writable-layer(container) gets wiped out when that container no longer exists

> Docker does not persist any data by default, read more about [storage and data persistence](https://docs.docker.com/storage/).

* **docker-compose**: A CLI tool used for local dev/test automation with YAML files for docker

> A docker compose file has versioning

### Infrastrucutre as Code (IaC)

### Infrastructure as Code (IaC) & all terminology

**- Idempotent** - Idempotent operation has no additional effect if it is called more than once with the same input parameters

**- Declarative** - Configuration explicitly specified in the code, uses scripting language like JSON & YAML

**- Imperative** - Configuration is implicit, you say what you want & rest is filled in. Used with programming languages Python, Ruby, Javascript

\*\*Declarative\*\* vs \*\*Imperative\*\*

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648481256131/Kss0T5nGW.png)

**- Provisioning** - Creating Infrastructure i.e. compute, storage containers, etc.

**- Deployment** - Deliver the application to provision the server. Deployment can be performed with AWS CodePipeline, Jenkins, GitHub Actions.

**- Orchestration** - Orchestration is coordinating with multiple systems & services. Orchestrations is a common term when working with microservices, containers & Kubernetes

**- Configuration Drift** - When provisioned infrastructure has unexpected configuration changes.

**- Mutable Infrastructure** - Mutable infrastructure can be updated, configured as per the requirement.

**- Immutable Infrastructure** - Immutable infrastructure is something that can never be modified once it is deployed.

**- Cloud Agnostic** - Cloud agnostic is a process where the company builds systems that aren't dependent on a specific provider

**- Execution Plan** - A manual review of what will add, change or destroy before you apply changes. Eg: `terraform apply`

Suggested Read 👉

* [Terraform Notes](https://notes.maheshrjl.com/terraform)
* [Ansible Notes](https://notes.maheshrjl.com/ansible)
