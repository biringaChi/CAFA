<h3 align = "center"> PAK-CPP: Program Analysis Framework for Continuous Performance Prediction </h3>
---

<p align="center"> <img src="..doc/PAK-CPP.svg" width="80%"> </p>

Official implementation of ```PAK-CPP: Program Analysis Framework for Continuous Performance Prediction``` to undergo review. For reviewer(s), please follow the instructions below to reproduce the results presented in the paper. 

## Abstract
> Software development teams establish elaborate continuous integration pipelines containing automated test cases to accelerate the development process of software. Automated tests help to verify the correctness of code modifications decreasing the response time to changing requirements. However, when the software teams do not track the performance impact of pending modifications, they may need to spend considerable time refactoring existing code. 
> 
> This paper presents PAK-CPP, a code analysis framework that provides continuous feedback on the performance impact of pending code updates. We design performance microbenchmarks by binding the execution test times of embedded automated test cases given a code update and map the execution test times to numerical statistics and distributional semantic code stylometry features used as input observations to predictive models tasked with code performance predictions. Our experiments achieved state-of-the-art performance in predicting the execution time performance of code updates to the software.

<hr>

> Note: Full paper details will be chronicled post-acceptance. PAK-CPP's [feature engineering](#feature-engineering) methods are the most transferable component of this work and are applicable in several other use cases beyond performance research. We leverage knowledge in code (SWE) and language (NLP) understanding to statistically and distributionally extract input features. 

<hr>
Artifact Author: Chidera Biringa
<hr>

## Installation
```
$ git clone https://github.com/biringaChi/PAK-CPP 
$ pip install -r requirements.txt 
$ cd src
```

## Problem Definition
<p align="center"> <img src="..doc/problem.svg" width="70%"> </p>

> Figure 1: Access and manipulation of a Linked-HashMap (LHM) using an Entry-set and Key-set. ESA and ESM denote Entry-set Access and Manipulation. KSA and KSM represent Key-set Access and Manipulation. Lines of code highlighted in green and red are LHM access and manipulation using  Entry-set and Key-set.

In this work, a code snippet or program is mediocre if it introduces a significant performance overhead to software and consequently skews the baseline resulting in an outlier performance. For example, consider the motivating example above. The snippet is a real-world fragment of a Java program, and it calculates the term frequency of more than 2000 elements in a linked hashmap (LHM). An LHM combines a hash table and a linked list. It ensures the predictable maintenance of elements in an iterable object. The peripheral difference between the snippets is in how the linked hashmap is accessed (ESA), (KSA), and manipulated (ESM), (KSM) using an ```entrySet()``` and ```keySet()``` respectively. On a test level, the ESA and ESM, and KSA and KSM versions of the code snippet execute in ```4 seconds``` and ```4 minutes``` as tested in our test environment, which means there is a ```6000% increase``` in ET. The difference decreases by half to ```3000%```, with an increase in the number of elements from 2,000 to 200,000. 

> Note: we are working under the assumption of an ```ideal``` software development environment, where external variables such as memory usage and network connectivity relatively outside the developer's control are operating at ```optimal``` levels.

## Microbenchmarking
Software performance microbenchmarking is a standardized procedure to experimentally analyze the execution time of non-functional components of the software, such as code snippets.  Test case results serve as the microbenchmarks, ground truth target variables fed to implemented predictive models for performance predictions.

## Rolling Predicitons & Deduplication
>Rolling Predictions. CCS: Current Code State. The features of a CCS given commit n a re trained on a regression model to predict the performance impact of CCS at commit n+1. Following that, PAK-CPP uses n+1 features in predicting n+2. PAK-CPP performs rolling predictions until n+n (the latest CCS). 
<p align="center"> <img src="..doc/roll.svg" width="60%"> </p>

## Code Stylometry Feature Engineering
This work leverages domain knowledge in software engineering (code stylometry) for feature extraction. Code stylometry is a source code's functional and non-functional characteristics. The table below details features of interest. An indispensable component of building predictive models is the transformation of text-based observations into numerical representations. Thus, post-feature extraction, we transform the aforementioned extracted features into numerical data points using our ```NSR``` and ```DSR``` algorithms. NSR and DSR are numerical statistics and distributional semantic representation methods. In NSR, we transformed features via frequency distribution, while DSR constitutes the adoption of unsupervised learning by mapping observations to vector space and deriving feature embeddings. 

---
### Taxonomy of Code Stylometry Features (CSF) -- Feature Selection 
 ```{Statements, Controls, Expressions}``` $\in$ ```Syntactic``` $\land$ ```{Invocations, Declarations}``` $\in$ ```Lexical``` 
| Class | Types | Brief Description | 
| --------------- | --------------- | --------------- |
| Statements | IfStatement, WhileStatement, DoStatement, AssertStatement, SwitchStatement, ForStatement, ContinueStatement, ReturnStatement, ThrowStatement, SynchronizedStatement, TryStatement, BreakStatement, BlockStatement, BinaryOperation, CatchClause | Dictates the behavior of a program under explicitly defined conditions |
| Controls | ForControl, EnhancedForControl 	  | Defines the repetition of instructions dependent on the satisfaction of requirements |
| Expressions | StatementExpression, TernaryExpression, LambdaExpression	  | Independent language entities with unique definitions |
| Invocations | SuperConstructorInvocation, MethodInvocation,  SuperMethodInvocation, SuperMemberReference, ExplicitConstructorInvocation, ArraySelector, AnnotationMethod, MethodReference | Defines the invocation of a program from another program |
| Declarations | TypeDeclaration, FieldDeclaration, MethodDeclaration, ConstructorDeclaration, PackageDeclaration, ClassDeclaration, EnumDeclaration, InterfaceDeclaration, AnnotationDeclaration, ConstantDeclaration, VariableDeclaration, LocalVariableDeclaration, EnumConstantDeclaration, VariableDeclarator  | Declares the existence of an entity in memory and assigns a value to that entity |

### CSF Representation
---
```"Representation learning, i.e., learning representations of the data that make it easier to extract useful information when building classifiers or other predictors" -- Bengio et al.``` <br />
The natural consequence of selecting features for predicting modeling is its representation. Ergo, our NSR and DSR algorithms. ```Pros of representation algorithms include:```
- Significant reduction in conventional vocabulary size due to application of domain (code stylometry) knowledge understanding. <br />
- Increased predictor training and prediction speed. <br />
- Significant reduction in sparse vectors.

#### Algorithm 1: Numerical Statistic Representation of CSF (NSR)
<p align="center"> <img src="..doc/nsr.png" width="50%"> </p>

#### Algorithm 2: Distributional Semantic Representation of CSF (NSR)
<p align="center"> <img src="..doc/dsr.png" width="50%"> </p>


## Reproducing Results in Paper
RQ1: Predictor Selection & Performance Analysis
```
$ TODO
```

RQ2: Throughput Analysis of CSF Selection & Representation
```
$ TODO
```

RQ3: Quant Comparison with SOTA
```
$ TODO
```
