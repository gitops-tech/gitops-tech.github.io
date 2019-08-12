# GitOps

Zitate
* gecoined von WeaveWorks
why this site? -> alles an einer Stelle, short and to the point

## Was ist GitOps?

GitOps ist eine Möglichkeit Continuous Deployment für Cloud Native Anwendungen
zu implementieren.

* Developer-centric

## Warum GitOps?

(eventuell hier schon Prinzipien)
Benefit -> Prinzip


## Wie funktioniert GitOps?

### Push

Bilder aus Seminararbeit, neues Layout mit wirklichem Flow

evtl. gifs

bei jedem Vorteile herausstellen für Vergleich

### Pull



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

### SVN?

### GitOps vs. DevOps?

### GitOps vs. NoOps?

### Is GitOps K8S only?

### Grenzen von GitOps?

jede Infrastruktur? was sind die requirements? when can I use GitOps?

### Ist mein Projekt ready for GitOps?

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