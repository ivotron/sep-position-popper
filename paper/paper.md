---
title: "Self-verifiable Experimentation Pipelines: An Alternative to Research Artifact Descriptions"
author: Ivo Jimenez and Carlos Maltzahn (_UC Santa Cruz_)
abstract: |
 We make the case for incorporating self-verifiable experimentation 
 pipelines in journals and conferences as a way of streamlining the 
 review process for code and data associated to academic articles.
---

# Introduction

Reproducibility is the cornerstone of the scientific method. Yet, in 
computational and data science domains, a gap exists between current 
practices and the ideal of having every new scientific discovery be 
_easily_ reproducible [@donoho_reproducible_2009]. Advances in 
computer science (CS) and software engineering slowly and painfully 
make their way into these domains---even in CS research itself
[@fursin_collective_2013 ; @collberg_repeatability_2016], paradoxically.

To address some of the reproducibility issues we face today, a growing 
number of Computer Science conferences and journals incorporate an 
artifact evaluation process in which authors of articles submit 
[artifact descriptions](http://ctuning.org/ae/submission.html) that 
are tested by a committee, in order to verify that experiments 
presented in a paper can be re-executed by others. In short, an 
artifact description is a 2-3 page narrative on how to replicate 
results, including steps that detail how to install software and how 
to re-execute experiments and analysis contained in a paper.

# Self-verifiable Pipelines {#sec:sep}

An alternative to the manual creation and verification of an Artifact 
Description (AD) is to use a continuous integration (CI) service[^cis] 
such as Jenkins[^jenkins] to ensure that the code (and data) 
associated to an article can be re-executed by others. If the 
execution and validation of an experimentation pipeline using a CI 
service, the URL pointing to the project on the CI server that holds 
execution logs, as well as the repository containing all the 
automation scripts, can potentially be used as an alternative to ADs. 
In other words, the repository containing the code for experimentation 
pipelines, and the associated CI project, serve both as a 
_Self-verifiable Experimentation Pipeline_ (SEP). Thus, instead of 
submitting manually written ADs, authors can submit a link to the code 
repository where scripts for their pipelines reside, along with a link 
to the CI server that executes them, and use this as the basis for 
evaluating the reproducibility of their work.

[^cis]: <https://en.wikipedia.org/wiki/Comparison_of_continuous_integration_software>
[^jenkins]: <https://jenkins.io>

While automating the execution of a pipeline can be done in many ways, 
in order for this approach to serve as an alternative to ADs, there 
are five high-level tasks that SEPs must carry out in every execution:

 1. **Code and data dependencies**. Code must reside on a version 
    control system (e.g. Github[^gh], Gitlab[^gl], etc.). If datasets 
    are used, then they should reside in a dataset management system 
    (datapackage, gitlfs, dataverse, etc.). The experimentation 
    pipelines must obtain the code/data from these services on every 
    execution.
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
    remote services (e.g. a scheduler). Many open-source tools can aid 
    in aggregating this information such as SOSReport[^sosreport] or 
    Facter[^facter].
 5. **Validation**. Scripts must verify that the output corroborates 
    the claims made on the article. For example, the pipeline might 
    check that the throughput of a system is within an expected 
    confidence interval (e.g. defined with respect to a baseline 
    obtained at runtime), or that a numerical computation is within 
    some expected bounds.

A list of Popper[^popper] [@jimenez_popper_2017] pipelines meeting the 
above criteria are available at Popper's official documentation 
website[^popperdoc].

[^popperdoc]: <https://popper.rtfd.io/en/latest/sections/examples.html>.
[^popper]: A SEP is a set of high-level requirements, rather than a 
detailed list of low-level requisites. Consequentially, there are many 
alternatives to create a SEP. While the examples we link to are of 
Popper pipelines, this article has the intention to apply, in general, 
to any type of automated approach.
[^gh]: <https://github.com>
[^gl]: <https://gitlab.com>
[^genilib]: <https://bitbucket.org/barnstorm/geni-lib>
[^sosreport]: <https://github.com/sosreport/sos>
[^facter]: <https://github.com/puppetlabs/facter>
[^terraform]: <https://terraform.io>

# Review Process {#sec:review}

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

Thus, incorporating SEPs to the peer-review process has two important 
properties:

  * Improved Experimentation Practices. Authors that consider 
    reproducibility as a first-class goal of their work increase the 
    confidence in their results.
  * Low-overhead Evaluations. With SEPs, the burden of ensuring that a 
    pipeline can be re-executed by others relies on the authors, wich 
    leverage the CI service as that third-person ensuring the help of 
    CI),

# References {.unnumbered}

\noindent
\vspace{-1em}
\setlength{\parindent}{-0.175in}
\setlength{\leftskip}{0.2in}
\setlength{\parskip}{0.5pt}
\fontsize{7pt}{8pt}\selectfont
