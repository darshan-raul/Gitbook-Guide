# Devops toolkit

This book is for everyone wanting to know more about the software development lifecycle starting from requirements and design, through development and testing all the way until deployment and post-deployment phases. We'll create the processes taking into account best practices developed by and for some of the biggest companies.

### continuous integration:

* Continuous integration \(CI\) usually refers to integrating, building, and testing code within the development environment. It requires developers to integrate code into a shared repository often.
* Getting code to the shared repository is not enough and we need to have a pipeline that, as a minimum, checks out the code and runs all the tests related, directly or indirectly, to the code corresponding to the repository. The result of the execution of the pipeline can be either red or green.
* The continuous integration pipeline should run on every commit or push. Unlike continuous delivery, continuous integration does not have a clearly defined goal of that pipeline.
* To gain maximum benefits, we should write tests in **test-driven development** \(TDD\) fashion

The most common flow is the following:

 • Pushing to the code repository • Static analysis • Pre-deployment testing • Packaging and deployment to the test environment • Post-deployment testing

