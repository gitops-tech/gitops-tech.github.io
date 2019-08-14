# GitOps

Zitate
* gecoined von WeaveWorks
why this site? -> alles an einer Stelle, short and to the point

## What is GitOps?

GitOps is a way of implementing Continuous Deployment for cloud native applications.

GitOps ist eine Möglichkeit Continuous Deployment für Cloud Native Anwendungen
zu implementieren.

* Developer-centric

## Warum GitOps?

(eventuell hier schon Prinzipien)
Benefit -> Prinzip


## How does GitOps work?

### Push

Bilder aus Seminararbeit, neues Layout mit wirklichem Flow

![Push based GitOps](images/push.png)

evtl. gifs

bei jedem Vorteile herausstellen für Vergleich

### Pull

![Pull based GitOps](images/pull.png)

## Prinzipien von GitOps

### Operations nur mit Git

Alle Änderungen am System müssen über Git durchgeführt werden.

Vorteile:

* Versionierung von allen Änderungen an der Infrastruktur
* Arbeiten mit bekannten Tools
* Reviews mit Kommentaren von Devs und Ops
* Reverts im Fehlerfall

### Deklarative Beschreibung des Systems

Die benötigte Infrastruktur des Systems muss deklarativ mit Infrastructure as
Code beschreibbar sein.

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

### GitOps vs. DevOps?

### GitOps vs. NoOps?

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

<img src="https://via.placeholder.com/80/FFFFFF" alt="Florian" width="80" style="float:left; padding: 12px" />

[Florian Beetz](#) is currently studying International Software Systems Science at University of Bamberg.

<div style="clear:left"/>

<img src="images/simon-avatar.png" alt="Simon" width="80" style="float:left; padding: 12px" />

[Simon Harrer](https://twitter.com/simonharrer) is a senior consultant at INNOQ. As part of a remote mob, he fights everyday out for simple solutions with domain-driven design, fitting architectures such as microservices or monoliths, and clean code in Java, Ruby or even JavaScript. Most recently, he wrote the book [Java by Comparison](https://java.by-comparison.com) that helps Java beginners to write cleaner code through before/after comparisons.



[Imprint and Privacy Policy](legal.md)

<script data-mode="hash" async defer src="https://cdn.simpleanalytics.io/hello.js"></script> 
<noscript><img src="https://api.simpleanalytics.io/hello.gif" alt=""></noscript>
