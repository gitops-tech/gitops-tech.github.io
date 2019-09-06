# GitOps

Since its inception in 2017 by [Weaveworks](https://www.weave.works/technologies/gitops/), GitOps has caused quite some fuss on Twitter and KubeCon.
This site aggregates the essence of GitOps to help clear up the confusion about the topic.


## What is GitOps?

GitOps is a way of implementing Continuous Deployment (CD) for cloud native applications.
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
With GitOps you have a complete history of what was deployed in any environment at any time in your repository.
Simon: stimmt nicht ganz, denn ob es wirklich deployed wurde ist ja nochmal was anderes. Du weisst lediglich, dass das vermutlich deplyoed wurde.
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
The application repository contains the source code of your application and the deployment manifests to deploy the application, while the environment repository contains all deployment manifests currently deployed in an environment. Simon: wie oben, aufpassen, da nur weil es im environment repo liegt, heisst das nicht, dass es wirklich korrekt deployed werden konnte. Oder was passiert, wenn das deployment fehlschlägt? Rollt dann der 'automated process' automatisch das git repo wieder zurück?

Of course, you can have multiple application repositories if you are working with microservices, or if your application consists of some other kind of independantly deployable components.
You can also manage multiple environments like you typically want for having multiple stages. For that, just use a branch per stage in the environment repository.


### Push-based GitOps

Push-based GitOps works with regular CI/CD tools, such as [Jenkins](https://jenkins.io/), [CircleCI](https://circleci.com/) or [Travis CI](https://travis-ci.org/).
The source code of your application lives inside the application repository along with your Kubernetes YAMLs needed to deploy your app.
Whenever your application code is updated, the CI pipeline is triggered, your application is built, and finally the environment repository is updated with new deployment descriptors.

Tip: You can also just store templates of your YAMLs to always set the currently build version correctly. Simon: store templates where? in the environment repository or the application repository?

![Push-based GitOps](images/push.png)

Changes to the environment repository trigger the Continuous Deployment pipeline. Simon: Unterschied CI zu CD pipeline? Wo liegt der genau?
This pipeline is responsible for applying all manifests in the environment repository to your infrastructure.

**Want to see how to set it up?** Check out [Google's Tutorial](https://cloud.google.com/kubernetes-engine/docs/tutorials/gitops-cloud-build) on how to set up Push-based GitOps with their Cloud Builds and GKE.


### Pull-based GitOps

Pull-based GitOps uses the same concepts as the push-based variant, but changes how the Continuous Deployment pipeline works.
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

**Want to see how to set it up?** Check out our [Case Study](case-study.md) about setting up Pull-based GitOps on Google's GKE.


### Working with multiple applications and environments

![Multiple applications and environments](images/multiple.png)

Simon: Evtl. wären auch Bilder spannend, die zeigen, wie es ist, wenn man mehrere Application Repos hat. Dann würde man ja das gleiche Environment Repo benutzen, oder? D.h. n applications 1 environment repo. Letztlich muss man auch aufzeigen, wie es sich bei unterschiedlichen environments (dev, stage, qa, prod) verhält. Evtl. als Sektion wie man das nun in einem Team umsetzen würde mit vielen Anwendungen und mindestens mal dev und prod als Umgebung. Das könnte interessant sein. 


## FAQ

### Is there also SVNOps?

In a way, yes. 
In principle, you can use any version control system you want.
One of the core ideas of GitOps is letting developers use the tools they are familiar with to operate your infrastructure. 
If you prefer SVN over Git, that's cool!
However, you may need to put more effort into finding tools that work for you or even write your own.
All available operators only work with Git repository - sorry!


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

No. Declarative Infrastructure as Code plays a huge role for implementing GitOps, but it's not just that.
GitOps takes the whole ecosystem and tooling around Git and applies it to infrastructure.
Continuous Deployment systems guarantee that the currently desired state of the infrastructure is deployed in the production environment.
Apart from that you gain all the benefits of code reviews, pull requests, and comments on changes for your infrastructure.


### I don't use Kubernetes. Can I still use GitOps?

Yes! GitOps is not limited to Kubernetes.
In principle, you can use any infrastructure that can be observed and described declaratively, and has Infrastructure as Code tools available.
However, currently most operators for pull-based GitOps are implemented with Kubernetes in mind.


### What are the limitations of GitOps?

GitOps doesn't really work well outside of the cloud.
Most cloud services can be managed with Infrastructure as Code tools, but the rack in your office probably can't.


### Is my project ready for GitOps?

If your project already lives in the cloud, the answer is most likely: Yes!
The cool thing about GitOps is that you don't need to write any code differently.
All you need to get started is infrastructure that can be managed with declarative Infrastructure as Code tools.


## Tooling

* [ArgoCD](https://argoproj.github.io/argo-cd/)
* [Weaveworks Flux](https://github.com/fluxcd/flux)
* ...

Einordnung? Pull/Push


## Weitere Resourcen

### Blog Posts

* [GitOps - Operations by Pull Request](https://www.weave.works/blog/gitops-operations-by-pull-request)
* [What is GitOps Really?](https://www.weave.works/blog/what-is-gitops-really)
* ...


## Authors

<img src="images/florian-avatar.png" alt="Florian" width="80" style="float:left; padding: 12px" />

[Florian Beetz](#) is currently studying International Software Systems Science at University of Bamberg.

<div style="clear:left"></div>

<img src="images/simon-avatar.png" alt="Simon" width="80" style="float:left; padding: 12px" />

[Simon Harrer](https://twitter.com/simonharrer) is a senior consultant at INNOQ. As part of a remote mob, he fights everyday out for simple solutions with domain-driven design, fitting architectures such as microservices or monoliths, and clean code in Java, Ruby or even JavaScript. Most recently, he wrote the book [Java by Comparison](https://java.by-comparison.com) that helps Java beginners to write cleaner code through before/after comparisons.



[Imprint and Privacy Policy](legal.md)

<script data-mode="hash" async defer src="https://cdn.simpleanalytics.io/hello.js"></script> 
<noscript><img src="https://api.simpleanalytics.io/hello.gif" alt=""></noscript>
