# GitOps

Since its inception in 2017 by [Weaveworks](https://www.weave.works/technologies/gitops/), GitOps has caused quite some fuss on Twitter and KubeCon.
This site aggregates the essence of GitOps to help clear up the confusion about the topic.


## What is GitOps?

GitOps is a way of implementing Continuous Deployment for cloud native applications.
It focuses on a developer-centric experience when operating infrastructure, by using tools developers are already familiar with, including Git and Continuous Integration and Delivery tools.

The core idea of GitOps is having an environment Git repository, that always contains declarative descriptions of the infrastructure currently deployed in the production environment.
All changes to the environment are done by pushing new descriptions of the infrastructure to the repository, where an automated process is triggered to make the production environment match the described state in the repository.

> GitOps: versioned CI/CD on top of declarative infrastructure. Stop scripting and start shipping. 
> 
> &mdash; [Kelsey Hightower](https://twitter.com/kelseyhightower/status/953638870888849408)


## Why should I use GitOps?

(eventuell hier schon Prinzipien)
Benefit -> Prinzip

### Deploy faster and more often

> When we say “high velocity” we mean that every product team can safely ship updates many times a day &mdash; deploy instantly, observe the results in real time, and use this feedback to roll forward or back.
>
> &mdash; [Weaveworks](https://www.weave.works/blog/gitops-high-velocity-cicd-for-kubernetes)

### Easy and fast error recovery

**To do**

> The Git record is then not just an audit log but also a transaction log. You can roll back & forth to any snapshot.
> 
> &mdash; [Alexis Richardson](https://twitter.com/monadic/status/1002502644798238721)


### Easier credential management

> kubectl is the new ssh. Limit access and only use it for deployments when better tooling is not available.
>
> &mdash; [Kelsey Hightower](https://twitter.com/kelseyhightower/status/1070413458045202433)


### Self-documenting deployments

Have you ever SSH'd into a server and wondered what's running there?
With GitOps, every change to any environment must happen through the environment repository.
You can always check out the master branch and get a complete description of what is deployed where plus the complete history of every change ever made to the system.


### Shared knowledge in teams

**To do**

Simon: maybe describe the case study in a separate markdown file on this (then two-page) microsite with lots of pictures. 


## How does GitOps work?

There are two ways how you can organize your Continuous Deployment process with Git repositories as the central element.


### Push-based GitOps

Bilder aus Seminararbeit, neues Layout mit wirklichem Flow

![Push-based GitOps](images/push.png)

evtl. gifs

bei jedem Vorteile herausstellen für Vergleich


### Pull-based GitOps

Simon: Evtl. wären auch Bilder spannend, die zeigen, wie es ist, wenn man mehrere Application Repos hat. Dann würde man ja das gleiche Environment Repo benutzen, oder? D.h. n applications 1 environment repo. Letztlich muss man auch aufzeigen, wie es sich bei unterschiedlichen environments (dev, stage, qa, prod) verhält. Evtl. als Sektion wie man das nun in einem Team umsetzen würde mit vielen Anwendungen und mindestens mal dev und prod als Umgebung. Das könnte interessant sein. 

![Pull-based GitOps](images/pull.png)


## Prinzipien von GitOps


### Operations nur mit Git

Alle Änderungen am System müssen über Git durchgeführt werden.

Vorteile:

* Versionierung von allen Änderungen an der Infrastruktur
* Arbeiten mit bekannten Tools
* Reviews mit Kommentaren von Devs und Ops
* Reverts im Fehlerfall

### Deklarative Beschreibung des Systems

Die benötigte Infrastruktur des Systems muss deklarativ mit Infrastructure as Code beschreibbar sein.

Vorteile:

* Idempotenz
* Einfacher zu verstehen, da IaC Tools die eigentliche Arbeit machen ("make it so!")

### Beobachtbarkeit des ganzen Systems

Nicht nur der Zustand der Software eines Systems muss beobachtbar sein, sondern
auch die Infrastruktur.

Vorteile:

* Vergleich mit dem gewollten Zustand

### Immutable Deployment Artifacts (?)

Alle Code Artefakte müssen unveränderbar sein.

* 

## FAQ

### Is there also SVNOps?

In a way, yes. 
In principle, you can use any version control system you want.
One of the core ideas of GitOps is letting developers use the tools they are familiar with to operate your infrastructure. 
If you prefer SVN over Git, that's cool!
However, you may need to put more effort into finding tools that work for you or even write your own.

### Should I hire GitOps engineers for my team now?

No! There are no GitOps engineers. GitOps is not a role (and neither is DevOps).
GitOps is a set of practices. You can look for a developer who has experience practicing GitOps &mdash; or simply let your developers try out those practices.


### We are already doing DevOps. What's the difference to GitOps?

### So, is GitOps basically NoOps?

### Is GitOps just versioned Infrastructure as Code?

No. Declarative IaC plays a huge role for implementing GitOps, but it's not just that.
GitOps takes the whole ecosystem and tooling around Git and applies it to infrastructure.
Continuous Deployment systems guarantee that the currently desired state of the infrastructure is deployed in the production environment.
Apart from that you gain all the benefits of code reviews, pull requests, and comments on changes for your infrastructure.


### I don't use Kubernetes. Can I still use GitOps?

### Grenzen von GitOps?

jede Infrastruktur? was sind die requirements? when can I use GitOps?

### Is my project ready for GitOps?

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
