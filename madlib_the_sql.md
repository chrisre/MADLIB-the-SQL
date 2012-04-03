# MADlib: An Open-Source Library for Scalable Analytics
*tl;dr: [**MADlib**](http://madlib.net) is an open-source library of scalable in-database machine learning, statistics and analytic methods.  It is modeled in part after the CRAN library for R, but built for scale and DBMS integration.  MADlib is a community effort funded via a significant people-power commitment from Greenplum, with technology transfer efforts underway from researchers at Berkeley, Florida and Wisconsin.  MADlib is now welcoming community contributions of analytic methods and ports to new DBMSs.*

## From Warehousing to Science
Back in 2008, I had the good fortune to work with a group of Big Data professionals to document emerging usage patterns in data analytics.  It was a broad group: a data scientist at a large social networking firm, a seasoned DBMS consultant formerly employed at a major Internet retailer, a pair of DBMS engine developers, and an academic.

The usage patterns we were seeing represented a shift from accountancy to analytics--from the cautious record-keeping of "Data Warehousing" to the open-ended, predictive task of "Data Science". This shift was turning many Data Warehousing tenets on their heads.  Rather than "architecting" an integrated permanent record that repelled data until it was well-conditioned, these groups were interested in fostering a data-centric computational "watering hole", where analysts could bring any kind of relevant data into a computational infrastructure, and experiment with ad-hoc integration and rich algorithmic analysis at very large scales. 

In response to the TLA-studded world of Data Warehousing (EDW), we dubbed this usage model **MAD**, to reflect 

  - the ***M**agnetic* aspect of an open shared infrastructure
  - the ***A**gile* design patterns used for modeling, loading and iterating on data, and
  - the ***D**eep* statistical models and algorithms being used.  

We wrote the [*MAD Skills* paper](http://www.vldb.org/pvldb/2/vldb09-219.pdf) to capture these practices in broad terms.  In addition to describing how databases were being used in this context, the paper also included a fairly technical section with a number of non-trivial analytics techniques adapted from the field, implemented via simple SQL excerpts.  

## MADlib (MAD Skills, the SQL)
When we released the MAD Skills paper, people got interested not only in its design aspects, but also in the promise of sophisticated statistical methods in SQL.  This interest came from many directions: DBMS customers were requesting it of consultants and vendors, and academics were increasingly publishing papers on in-database analytics.  What was missing was a software framework to harness the energy of the community, and connect the various interested constituencies.  

To this end we began to build [**MADlib**](http://madlib.net), a free, open source library of SQL-based algorithms for machine learning, data mining and statistics. The methods in MADlib are designed both for in- or out-of-core execution, and for the shared-nothing, ``scale-out'' parallelism offered by modern parallel database engines, ensuring that computation is done close to the data.  The core functionality is written in declarative SQL statements, which orchestrate data movement to and from disk, and across networked machines.  Single-node inner loops take advantage of SQL extensibility to call out to  high-performance math libraries (currently, [Eigen](http://eigen.tuxfamily.org/)) in user-defined scalar and aggregate functions.  At the highest level, tasks that require iteration and/or structure definition are coded in Python driver routines, which are used only to kick off the data-rich computations that happen within the database engine.

The primary goal of the MADlib open source project is to accelerate innovation and technology transfer in the Data Science community via a shared library of scalable in-database analytics, much as the [CRAN library](http://cran.r-project.org/) serves the R community.  Unlike CRAN, which is customized to the R analytics tool, we hope that MADlib's grounding in standard SQL can lead to community ports to a variety of parallel database engines. 

## Why Databases?
Big Data is on everyone's mind today, and competition for extracting value from data has become increasingly refined--especially in fiercely competitive application domains like online advertising or politics.  It is of course important to target "typical" people (customers, voters) that could be captured by a small sample or data extract that fits in a tool like R or Matlab.  But winning today requires extracting advantages that can only be found in the long tail of "special interests", a practice known as "microtargeting", "hypertargeting" or "narrowcasting". Studying the long tail requires acquiring and analyzing all the data.

As momentum has been gathering around the possibilities of Big Data, efforts have begun to
implement statistical methods in various scalable processing platforms. For example, the open source [Mahout](http://mahout.apache.org/) project aims to implement machine learning tools within Hadoop MapReduce, harnessing interest in both academia and industry. This seems like one promising approach: even traditional database players like Oracle and Microsoft are now pushing Hadoop solutions.

At the same time that the Hadoop ecosystem has been evolving, the SQL-based analytics ecosystem has continued to grow, and large volumes of valuable data will pour into
SQL systems for many years to come. There is a rich
ecosystem of tools, know-how, and organizational requirements that
encourage this.  For these cases, it is helpful to push statistical methods into the DBMS. MADlib currently targets this environment of in-database analytics.

It is no surprise to database researchers that a massively parallel DBMS can   be a powerful platform for dataflow programming of sophisticated analytic algorithms.  Research on sophisticated in-database analytics has been growing in recent years, in part as an offshoot of work on Probabilistic Databases. Education may be shifting as well: this year in my own [CS186](https://sites.google.com/a/cs.berkeley.edu/cs186-s12/) database course the students not only have to write traditional SQL queries, they also had to [implement Betweenness Centrality](https://github.com/cs186/sp12/blob/master/hw2/README.md).

From a marketplace perspective, many shops deploy both Hadoop and DBMS platforms, and desire analytics libraries for each.  From an educational perspective, it is useful for students to learn to write non-trivial algorithms in both frameworks.  A reasonable strategy for the community is to foster analytics work in both SQL and Hadoop environments, while exploring new scalable analytics architectures at the same time ([GraphLab](http://graphlab.org/), [SciDB](http://www.scidb.org), [Spark](http://www.cs.berkeley.edu/~matei/spark/), [ScalOps](http://cs.markusweimer.com/2011/11/21/machine-learning-in-scalops-a-higher-order-cloud-computing-language/), etc.).


## Why Open Source?
From the beginning, MADlib was designed as an open source project with
corporate backing, rather than a closed-source corporate effort with
academic consulting. This decision was motivated by a number of
factors, including the following:

  - **The benefits of customization**: Statistical methods are rarely used as turnkey solutions.  It's typical for data scientists to want to modify and adapt canonical models and methods to their own purposes.  Open source has major advantages in that context, and enables useful modifications to be shared back to the benefit of the entire community.
  - **Valuable data vs. valuable software**:  For many new companies (e.g. social networks), their corporate value is captured in their data assets, not their software.  These companies benefit from open source adoption and improvement of their software.  But open source efforts can also be synergistic for commercial software vendors, as recent announcements have demonstrated.  Meanwhile, for most database system vendors, their core competency is not in statistical methods, but rather in the engines that support those methods, and the service industry that evolves around them.  For these vendors, involvement and expertise with an open source library like MADlib is an opportunity to expand their functionality and service offerings. 
  - **Closing the research-to-adoption loop**:  Very few traditional database customers have the capacity to develop significant in-house research into computing or data science. 
    On the other hand, it is hard for academics doing computing research to understand and influence the way that analytic processes are done in the field.  An open source project like MADlib has the potential to connect these constituencies in a concrete way.
  - **Leveling the playing field, encouraging innovation**:  DBMS vendors offer various proprietary data mining toolkits consisting of textbook algorithms.  It is hard to assess their relative merits.  Meanwhile, Internet companies have been busily building machine learning code at scale, but their code is not well packaged for reuse.  (At a recent meeting, I asked leaders at two major Internet companies if they would contribute standard machine learning code to the community.  Their joint response was that they would if they could, but the code was not in shape to be distributed.)  None of the existing Big Data analytics libraries has demonstrated the vibrancy and breadth we see in the open source community surrounding R and its CRAN package. The goal of MADlib is to fill this gap: remove opportunities for proprietary *FUD*,
     bring the database community up to a baseline level of competence on standard statistical algorithms,  and help focus a large community on innovation and technology transfer.

##A Model for Open Source Collaboration

The design of MADlib comes at a time when the connections between
open source software and academic research seem particularly frayed.
MADlib is designed in part as an experiment in binding these
communities more tightly, to face current realities in software
development.

In previous decades, many important open source packages originated in universities and evolved into significant commercial products.  Think of Ingres and Postgres, BSD UNIX and Mach, the X Window and Kerberos tool kits.  All were characterized by aggressive application of new research ideas, captured in workable but fairly raw public releases that matured slowly with the help of communities outside the university.  

Today, we expect successful open source projects to be mature: comparable to commercial products.  To achieve this level of
maturity, most successful open source projects have one or more major
corporate backers who pay some number of committers and provide
professional support for Quality Assurance (QA).  This kind of investment is typically
made in familiar software packages, not academic research projects.  Many of the most popular examples--Hadoop, Linux,
OpenOffice--began as efforts to build open source alternatives to well-identified,
pre-existing commercial efforts. 

MADlib is making an explicit effort to explore a new model for industry support of academic research via open source.  Many academic research projects are generously supported by financial grants and gifts from companies.  In MADlib, the corporate donation has largely consisted of a commitment to allocate significant professional software engineering time to bootstrap an open-source sandbox for academic research and tech transfer to practice. 
This leverages a strength of industry that is not easily replicated by government and other non-profit funding sources.  Companies can recruit high-quality, experienced software
engineers with the attraction of well-compensated, long-term career
paths.  Equally important, software shops can offer an entire software
engineering pipeline that cannot be replicated on campus:
this includes QA processes and QA engineering staff.  The hope is that the corporate staffing of research projects like MADlib can enable more impactful
academic open source research, and speed technology transfer to industry. 

##MADlib Status

<a name="table1">
<img src="methods.png" width="400"></a>

MADlib is still young, at Version 0.3.  The initial versions focused on establishing infrastructure and a baseline of textbook methods (Table 1).  Most methods were chosen because they were frequently requested from customers we met through contacts at Greenplum.  More recently, we made a point of validating MADlib as a research vehicle, by fostering a small number of university groups who were working in the area to experiment with the platform and get their code disseminated.

MADlib is hosted publicly at [github](http://github.com/madlib/madlib), and readers are encouraged to
browse the code and documentation via the [MADlib website](http://madlib.net). The initial MADlib codebase reflects contributions from both industry (Greenplum) and academia (UC Berkeley, the University of Wisconsin, and the University of Florida).  Code
management and Quality Assurance efforts have been contributed by
Greenplum.  The team has written a [technical report](http://www.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-38.html) that expands on this post. 

At this time, the project is ready to consider
contributions from additional parties, including both new methods and
ports to new platforms.  Like any serious open source project, that process will be managed with an eye toward code quality.  We hope that members of the SIGMOD community will find it worthwhile to join the MADlib effort, working to collectively develop and refine production-quality open-source code in this important new area.