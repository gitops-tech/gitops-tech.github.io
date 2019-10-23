# GitOps

Since its inception in 2017 by [Weaveworks](https://www.weave.works/technologies/gitops/), GitOps has caused quite some fuss on Twitter and KubeCon.
This site aggregates the essence of GitOps to help clear up the confusion about the topic.


## What is GitOps?

GitOps is a way of implementing Continuous Deployment for cloud native applications.
It focuses on a developer-centric experience when operating infrastructure, by using tools developers are already familiar with, including Git and Continuous Deployment tools.

The core idea of GitOps is having a Git repository that always contains declarative descriptions of the infrastructure currently desired in the production environment and an automated process to make the production environment match the described state in the repository.
If you want to deploy a new application or update an existing one, you only need to update the repository - the automated process handles everything else. 
It's like having cruise control for managing your applications in production.

> GitOps: versioned CI/CD on top of declarative infrastructure. Stop scripting and start shipping. 
> 
> &mdash; [Kelsey Hightower](https://twitter.com/kelseyhightower/status/953638870888849408)


## Why should I use GitOps?

### Deploy faster and more often

Okay, to be fair, probably every Continuous Deployment technology promises to make deploying faster and allows you to deploy more often.
What is unique about GitOps is that you don't have to switch tools for deploying your application.
Everything happens in the version control system you use for developing the application anyways.

> When we say “high velocity” we mean that every product team can safely ship updates many times a day &mdash; deploy instantly, observe the results in real time, and use this feedback to roll forward or back.
>
> &mdash; [Weaveworks](https://www.weave.works/blog/gitops-high-velocity-cicd-for-kubernetes)

### Easy and fast error recovery

Oh no! Your production environment is down!
With GitOps you have a complete history of how your environment changed over time.
This makes error recovery as easy as issuing a `git revert` and watching your environment being restored.

> The Git record is then not just an audit log but also a transaction log. You can roll back & forth to any snapshot.
> 
> &mdash; [Alexis Richardson](https://twitter.com/monadic/status/1002502644798238721)


### Easier credential management

GitOps allows you to manage deployments completely from inside your cluster.
For that, your cluster only needs access to your repository and image registry.
That's it.
You don't have to give your developers direct access to the cluster.

> kubectl is the new ssh. Limit access and only use it for deployments when better tooling is not available.
>
> &mdash; [Kelsey Hightower](https://twitter.com/kelseyhightower/status/1070413458045202433)


### Self-documenting deployments

Have you ever SSH'd into a server and wondered what's running there?
With GitOps, every change to any environment must happen through the repository.
You can always check out the master branch and get a complete description of what is deployed where plus the complete history of every change ever made to the system.


### Shared knowledge in teams

Using Git to store complete descriptions of your deployed infrastructure allows everybody in your team to check out its evolution over time.
With great commit messages everybody can reproduce the thought process of changing infrastructure and also easily find examples of how to set up new systems.

> GitOps is the best thing since configuration as code. Git changed how we collaborate, but declarative configuration is the key to dealing with infrastructure at scale, and sets the stage for the next generation of management tools.
> 
> &mdash; [Kelsey Hightower](https://twitter.com/kelseyhightower/status/1164192321891528704)


## How does GitOps work?

There are two ways how you can organize your Continuous Deployment process with Git repositories as the central element: Push-based GitOps and Pull-based GitOps.
They both have in common, that they use at least two repositories: the application repository and the environment repository.
The application repository contains the source code of your application and the deployment manifests to deploy the application, while the environment repository contains all deployment manifests of the currently desired infrastructure in an environment.
The difference between the two types of GitOps, is how it is ensured, that the environment actually resembles the desired infrastructure.


### Push-based GitOps

Push-based GitOps works with regular CI/CD tools, such as [Jenkins](https://jenkins.io/), [CircleCI](https://circleci.com/) or [Travis CI](https://travis-ci.org/).
The source code of your application lives inside the application repository along with your Kubernetes YAMLs needed to deploy your app.
Whenever your application code is updated, the build pipeline is triggered, which builds your container images, and finally the environment repository is updated with new deployment descriptors.

Tip: You can also just store templates of your YAMLs in your application repository.
When a new version is built, you can use the template to generate the YAML in your environment repository.

![Push-based GitOps](images/push.png)

Changes to the environment repository trigger the deployment pipeline.
This pipeline is responsible for applying all manifests in the environment repository to your infrastructure.

One important thing to keep in mind when using this approach, is that the deployment pipeline *only* is triggered when your environment repository changes.
It can not automatically notice any deviations of the environment and its desired state.
This means, you need some way of monitoring in place, so that you can intervene if your environment doesn't match what is described in the environment repository.

**Want to see how to set it up?** Check out [Google's Tutorial](https://cloud.google.com/kubernetes-engine/docs/tutorials/gitops-cloud-build) on how to set up Push-based GitOps with their Cloud Builds and GKE.


### Pull-based GitOps

Pull-based GitOps uses the same concepts as the push-based variant, but changes how the deployment pipeline works.
Traditional CI/CD pipelines are triggered by an external event, for example when new code is pushed to an application repository.
With this approach, the *operator* is introduced.
It takes over the role of the pipeline by continuously comparing the desired state in the environment repository with the actual state in the deployed infrastructure.
Whenever differences are noticed, the operator updates the infrastructure to match the environment repository.
Additionally, when working with Kubernetes clusters, the image registry can be monitored to find new versions of images and deploy them to the cluster.

![Pull-based GitOps](images/pull.png)

Just like push-based GitOps, this variant updates the environment whenever the environment repository changes.
However, with the operator, changes can also be noticed in the other direction.
Whenever the deployed infrastructure changes in any way not described in the environment repository, these changes are reverted.
This ensures that all changes are made tracable in the Git log, by making all direct changes to the cluster impossible.

This change in direction solves the problem of push-based GitOps, where the environment is only updated when the environment repository is updated.
However, this doesn't mean you can completely do without any monitoring in place.
Most operators support sending mails or Slack notifications if it can not bring the environment to the desired state for any reason, for example if it can not pull a container image.
Additionally, you probably should set up monitoring for the operator itself, as there is no longer any automated deployment process without it.

<!-- **Want to see how to set it up?** Check out our [Case Study](case-study.md) about setting up Pull-based GitOps on Google's GKE. -->


### Working with multiple applications and environments

Of course working with just one application repository and only one environment is not realistic for most applications.
When you are using a microservices architecture, you probably want to keep each service in its own repository.

GitOps can also handle such a use case.
You can always just set up multiple build pipelines that update the environment repository.
From there on the regular automated GitOps workflow kicks in and deploys all parts of your application.

![Multiple applications and environments](images/multiple.png)

Managing multiple environments with GitOps can be done by just using separate branches in the environment repository.
You can set up the operator or the deployment pipeline to react to changes on one branch by deploying to the production environment and another to deploy to staging.


## FAQ

### Is there also SVNOps?

In a way, yes. 
In principle, you can use any version control system you want.
One of the core ideas of GitOps is letting developers use the tools they are familiar with to operate your infrastructure. 
If you prefer SVN over Git, that's cool!
However, you may need to put more effort into finding tools that work for you or even write your own.
All available operators only work with Git repository &mdash; sorry!


### Should I hire GitOps engineers for my team now?

No! There are no GitOps engineers. GitOps is not a role (and neither is DevOps).
GitOps is a set of practices. You can look for a developer who has experience practicing GitOps &mdash; or simply let your developers try out those practices.


### We are already doing DevOps. What's the difference to GitOps?

DevOps is all about the cultural change in an organization to make people work better together.
GitOps is a technique to implement Continuous Delivery.
While DevOps and GitOps share principles like automation and self-serviced infrastructure, it doesn't really make sense to compare them.
However, these shared principles certainly make it easier to adopt a GitOps workflow when you are already actively employing DevOps techniques.


### So, is GitOps basically NoOps?

You can use GitOps to implement NoOps, but it doesn't automatically make all operations tasks obsolete.
If you are using cloud resources anyway, GitOps can be used to automate those.
Typically, however, some part of the infrastructure like the network configuration or the Kubernetes cluster you use isn't managed by yourself decentrally but rather managed centrallly by some operations team.
So operations never really goes away.


### Is GitOps just versioned Infrastructure as Code?

> Is GitOps just a new name for Infra as Code?
> 
> &mdash; [sholom](https://twitter.com/sholom/status/1173613576696795136)

No. Declarative Infrastructure as Code plays a huge role for implementing GitOps, but it's not just that.
GitOps takes the whole ecosystem and tooling around Git and applies it to infrastructure.
Continuous Deployment systems guarantee that the currently desired state of the infrastructure is deployed in the production environment.
Apart from that you gain all the benefits of code reviews, pull requests, and comments on changes for your infrastructure.


### I don't use Kubernetes. Can I still use GitOps?

Yes! GitOps is not limited to Kubernetes.
In principle, you can use any infrastructure that can be observed and described declaratively, and has Infrastructure as Code tools available.
However, currently most operators for pull-based GitOps are implemented with Kubernetes in mind.


### What are the limitations of GitOps?

GitOps does not work with infrastructure that can not be observed or decaratively described.
Kubernetes comes with both out of the box, but the same can also be achieved for other cloud resources with infrastructure as code tools like Terraform.


### Is my project ready for GitOps?

If your project already lives in the cloud or in a Kubernetes cluster, the answer is most likely: Yes!
The cool thing about GitOps is that you don't need to write any code differently.
All you need to get started is infrastructure that can be managed with declarative Infrastructure as Code tools.


## Tooling

* [ArgoCD](https://argoproj.github.io/argo-cd/): A GitOps operator for Kubernetes with a web interface
* [Flux](https://github.com/fluxcd/flux): The GitOps Kubernetes operator by the creators of GitOps &mdash; [Weaveworks](https://www.weave.works/technologies/gitops/)
* [Gitkube](https://gitkube.sh): A tool for building and deploying docker images on Kubernetes using `git push`
* [JenkinsX](https://jenkins-x.io/): Continuous Delivery on Kubernetes with built-in GitOps
* [Terragrunt](https://github.com/gruntwork-io/terragrunt): A wrapper for [Terraform](https://www.terraform.io/) for keeping configurations DRY, and managing remote state
* [WKSctl](https://github.com/weaveworks/wksctl): A tool for Kubernetes cluster configuration management based on GitOps principles

Also check out Weavework's [Awesome-GitOps](https://github.com/weaveworks/awesome-gitops).

## Resources

### Blog Posts and Social Media

* [An Inside Look at GitOps](https://devops.com/an-inside-look-at-gitops/)
* [GitOps - Operations by Pull Request](https://www.weave.works/blog/gitops-operations-by-pull-request)
* [GitOps: What, Why, and How.](https://www.reddit.com/r/kubernetes/comments/dc8bfd/gitops_what_why_and_how/)
* [What Is GitOps and Why It Might Be The Next Big Thing for DevOps](https://thenewstack.io/what-is-gitops-and-why-it-might-be-the-next-big-thing-for-devops/)
* [What is GitOps Really?](https://www.weave.works/blog/what-is-gitops-really)

### Talks

* [GitOps - Operations by Pull Request [B] - Alexis Richardson, Weaveworks & William Denniss, Google](https://www.youtube.com/watch?v=BSqE2RqctNs)
* [Tutorial: Hands-on Gitops - Brice Fernandes, Weaveworks](https://www.youtube.com/watch?v=0SFTaAuOzsI)
* [What is GitOps? Next level delivery with Flux & Kubernetes by Rafał Lewandowski](https://www.youtube.com/watch?v=5zt-jzKHwX8)


## Authors

<img src="images/florian-avatar.png" alt="Florian" width="80" style="float:left; padding: 12px" />

[Florian Beetz](https://www.linkedin.com/in/florian-beetz-251048191/) is currently studying International Software Systems Science at University of Bamberg.
He is interested in cloud computing, clean code, and software engineering techniques.
In his free time, he likes to go rock climbing.

<div style="clear:left"></div>

<img src="images/simon-avatar.png" alt="Simon" width="80" style="float:left; padding: 12px" />

[Simon Harrer](https://twitter.com/simonharrer) is a senior consultant at INNOQ. As part of a [remote mob](https://remotemobprogramming.org), he fights everyday out for simple solutions with domain-driven design, fitting architectures such as microservices or monoliths, and clean code in Java, Ruby or even JavaScript. Most recently, he wrote the book [Java by Comparison](https://java.by-comparison.com) that helps Java beginners to write cleaner code through before/after comparisons.



[Imprint and Privacy Policy](legal.md)
