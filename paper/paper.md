---
title: "Self-verifiable Experimentation Pipelines"
subtitle: "An Alternative to Research Artifact Descriptions"
author: Ivo Jimenez and Carlos Maltzahn (UC Santa Cruz)
abstract: |
 We make the case for incorporating self-verifiable experimentation 
 pipelines in journals and conferences as a way of streamlining the 
 review process for code and data associated to academic articles.
---

# Introduction

Reproducibility is the cornerstone of the scientific method. Yet, in 
computer, computational and data science domains, a gap exists between 
current practices and the ideal of having every new scientific 
discovery be _easily_ reproducible [@donoho_reproducible_2009]. 
Advances in computer science (CS) and software engineering slowly and 
painfully make their way into these domains---even in CS research 
itself
[@fursin_collective_2013 ; @collberg_repeatability_2016], paradoxically.

To address some of the reproducibility issues we face today, a growing 
number of CS conferences and journals incorporate an artifact 
evaluation process in which authors submit artifacts and 
descriptions[^ad] (ADs) that are tested by a committee, in order to 
verify that experiments presented in a paper can be re-executed by 
others. In short, an artifact description is a 2-3 page narrative on 
how to replicate results by making us of the submitted artifacts (e.g. 
source code and data), including steps that detail how to install 
software and how to re-execute experiments and analysis contained in a 
paper.

[^ad]: <http://ctuning.org/ae/submission.html>

While an AD can serve as the basis for evaluating the reproducibility 
of a scientific exploration carried out as part of an article, it 
gives rise to some shortcomings during the artifact evaluation process 
[@fursin_cgoppopp17_2017]:

  * Too many ad-hoc scripts.
  * Intense schedule and little time for rebuttals.
  * Sometimes hard to find evaluators with the appropriate skills or 
    access to proprietary software or custom hardware.
  * Lack of common workflows.
  * No common formats and APIs (benchmarks, datasets, tools).
  * Difficult to reproduce empirical results across a diverse software 
    and hardware, and for distinct inputs.

In this short position paper, we introduce self-verifiable 
experimentation pipelines (SEP) and make the case for using them to 
address the challenges outlined above. In short, a SEP is a set of 
scripts associated to an experiment, that are executed by an 
automation server. Instead of having authors submit artifacts and ADs, 
authors submit a link to an automation server that executes a SEP 
without any manual intervention.

# Self-verifiable Pipelines {#sec:sep}

An alternative to the manual creation and verification of an Artifact 
Description (AD) is to use a continuous integration (CI) service[^cis] 
such as Jenkins[^jenkins] to ensure that the code (and data) 
associated to an article can be re-executed by others. If authors use 
a CI service to automate the execution and validation of their 
experimentation pipelines, the URL pointing to the project on the CI 
server that holds execution logs, as well as the repository containing 
all the automation scripts, can be used as the basis for evaluating 
the reproducibility of the code and data associated to an article. In 
other words, the repository containing the code for experimentation 
pipelines and the associated CI project serve both as a 
_Self-verifiable Experimentation Pipeline_ (SEP). Thus, instead of 
submitting manually written ADs, authors can submit a link to the code 
repository where scripts for their pipelines reside, along with a link 
to the CI server that executes them, and use this as the basis for 
evaluating the reproducibility of their work.

[^cis]: <http://en.wikipedia.org/?curid=22903426>
[^jenkins]: <https://jenkins.io>

While automating the execution of a pipeline can be done in many ways, 
in order for this approach to serve as an alternative to ADs, there 
are five high-level tasks that SEPs must carry out in every execution:

 1. **Code and data dependencies**. Code must reside on a version 
    control system (e.g. Github[^gh], Gitlab[^gl], etc.). If datasets 
    are used, then they should reside in a dataset management system 
    (Zenodo[^zenodo], CKAN[^ckan], Datapackages[^datapackages], etc.). 
    The experimentation pipelines must obtain the code/data from these 
    services on every execution.
 2. **Setup**. The pipeline should build and deploy the code under 
    test. For example, if a pipeline is using containers or VMs to 
    package their code, the pipeline should build the container/VM 
    images prior to executing them. The goal of this is to verify that 
    all the code and 3rd party dependencies are available at the time 
    a pipeline runs, and that software can be build correctly.
 3. **Resource allocation**. If a pipeline requires a cluster or 
    custom hardware to reproduce results, resource allocation must be 
    done as part of the execution of the pipeline. This allocation can 
    be static or dynamic. For example, if an experiment runs on custom 
    hardware, the pipeline can statically allocate (i.e. hardcode 
    IP/hostnames) the machines where the code under study runs (e.g. 
    GPU/FPGA nodes). Alternatively, a pipeline can dynamically 
    allocate nodes on CloudLab [@ricci_introducing_2014], Grid500K 
    [@bolze_grid5000_2006], Chameleon [@mambretti_next_2015], or a 
    public cloud provider. These services typically offer 
    infrastructure automation tools such as Geni-lib[^genilib] 
    (Cloudlab), Enos [@cherrueau_holistic_2017] (Chameleon), SLURM 
    [@yoo_slurm_2003] (HPC centers), or Terraform[^terraform] (AWS, 
    GCP, etc.). All these tools can be used to automate 
    infrastructure-related tasks.
 4. **Environment capture**. Capture information about the runtime 
    environment. For example, hardware description, OS, system 
    packages (i.e. software installed by system administrators), 
    information about remote services (e.g. version and state of a 
    batch scheduler), etc. Open-source tools such as 
    SOSReport[^sosreport] or Facter[^facter] can aid in aggregating 
    this information.
 5. **Validation**. Scripts must verify that the output corroborates 
    the claims made on the article. For example, the pipeline might 
    check that the throughput of a system is within an expected 
    confidence interval (e.g. defined with respect to a baseline 
    obtained at runtime), or that a numerical computation is within 
    some expected bounds. This can be done with domain-specific tools 
    [@jimenez_aver_2016], or generic data analytics stacks such as 
    Pandas [@mckinney_data_2010] or R [@r.coreteam_r_2013].

A list of Popper [@jimenez_popper_2017] pipelines meeting the above 
criteria are available at Popper's official documentation 
website[^popperdoc].

> **NOTE**: A SEP is a set of high-level requirements, rather than a 
> detailed list of low-level requisites. Consequentially, there are 
> many alternatives for creating a SEP. While the examples we link to 
> are of Popper pipelines, this article has the intention to apply, in 
> general, to any type of automated approach.

[^popperdoc]: <http://bit.ly/popper-examples>.
[^gh]: <https://github.com>
[^gl]: <https://gitlab.com>
[^zenodo]: <https://zenodo.org>
[^ckan]: <https://ckan.org/>
[^datapackages]: <https://frictionlessdata.io/data-packages/>
[^genilib]: <https://bitbucket.org/barnstorm/geni-lib>
[^sosreport]: <https://github.com/sosreport/sos>
[^facter]: <https://github.com/puppetlabs/facter>
[^terraform]: <https://terraform.io>

# SEP-based Reproducibility Evaluation {#sec:review}

We now illustrate how the peer-review process might work for a 
conference or journal that incorporates SEPs as a way of having 
authors demonstrate the reproducibility of their results. The process 
might look like this:

 1. Authors create SEPs as part of their work.
 2. Authors submit link(s) to the CI service that automatically 
    executes the experimentation pipeline(s) associated to their 
    submission.
 3. Reviewers check logs of CIs to verify that the 5 components 
    described in @Sec:sep are met.

Thus, incorporating SEPs to the peer-review process has several 
important outcomes:

 1. **Better Experimentation Practices**. Authors that consider 
    reproducibility as a first-class goal of their work increase the 
    confidence in their results.

 2. **Low-overhead Evaluations**. With SEPs, the burden of ensuring 
    that a pipeline can be re-executed by others relies on the 
    authors, who leverage the CI service as the third party that is in 
    charge of checking all their scripts and dependencies work as they 
    should. As a consequence, all the experiment-specific knowledge in 
    the scripts that the CI server executes is codified and automated. 
    This in turn results in removing the need for experts that can 
    re-build from scratch what an author originally ran. All a 
    reviewer has to do is to audit that the automation server is 
    indeed going through the steps outlined in @Sec:sep. An optional 
    requirement might be the ability for reviewers to trigger a new 
    execution of the experiment, assuming resources are available, and 
    there are no security or IP restrictions.

 4. **Common high-level workflow**. Since the specification of what a 
    pipeline is abstract, both authors and reviewers share the same 
    mental mode when it comes to understanding what a scientific 
    exploration is doing.

# References {.unnumbered}

\noindent
\vspace{-1em}
\setlength{\parindent}{-0.175in}
\setlength{\leftskip}{0.2in}
\setlength{\parskip}{0.5pt}
\fontsize{7pt}{8pt}\selectfont
