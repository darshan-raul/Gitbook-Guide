# Sagemaker

## Features

Use [Amazon SageMaker Canvas](https://aws.amazon.com/sagemaker/canvas/) to automatically explore different solutions and find the best model for your given dataset.

[Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-use-augmented-ai-a2i-human-review-loops.html)

Build the workflows required for human review of ML predictions. Amazon A2I brings human review to all developers, removing the <mark style="color:purple;">undifferentiated heavy lifting</mark> associated with building human review systems or managing large numbers of human reviewers.

[AutoML step](https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-steps.html)

Create an AutoML job to automatically train a model in Pipelines.

[SageMaker Autopilot](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development.html)

Users without machine learning knowledge can quickly build classification and regression models.

[Batch Transform](https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html)

Preprocess datasets, run inference when you don't need a persistent endpoint, and associate input records with inferences to assist the interpretation of results.

[SageMaker Clarify](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html#clarify-fairness-and-explainability)

SageMaker Clarif<mark style="color:purple;">y automatically evaluates foundation models for your generative AI use case with metrics such as accuracy, robustness, and toxicity to support your responsible AI initiative.</mark> SageMaker Clarify explains how input features contribute to your model predictions during model development and inference. Evaluate your FM during customization using automatic and human-based evaluations.

Improve your machine learning models by <mark style="color:purple;">detecting potential bias</mark> and help explain the predictions that models make. You can use Amazon SageMaker Clarify to understand fairness and model explainability and to explain and detect bias in your models. You can configure an SageMaker Clarify processing job to compute bias metrics and feature attributions and generate reports for model explainability. SageMaker Clarify processing jobs are implemented using a specialized SageMaker Clarify container image.

[Collaboration with shared spaces](https://docs.aws.amazon.com/sagemaker/latest/dg/domain-space.html)

A shared space consists of a shared JupyterServer application and a shared directory. All user profiles in a Amazon SageMaker AI domain have access to all shared spaces in the domain.

[SageMaker Data Wrangler](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler.html)

Import, analyze, prepare, and featurize data in SageMaker Studio. You can integrate Data Wrangler into your machine learning workflows to simplify and streamline data pre-processing and feature engineering using little to no coding. You can also add your own Python scripts and transformations to customize your data prep workflow.

[Data Wrangler data preparation widget](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-interactively-prepare-data-notebook.html)

Interact with your data, get visualizations, explore actionable insights, and fix data quality issues.

[SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/train-debugger.html)

Inspect training parameters and data throughout the training process. Automatically detect and alert users to commonly occurring errors such as parameter values getting too large or small.

[SageMaker Edge Manager](https://docs.aws.amazon.com/sagemaker/latest/dg/edge.html)

Optimize custom models for edge devices, create and manage fleets and run models with an efficient runtime.

[SageMaker Experiments](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)

Experiment management and tracking. You can use the tracked data to reconstruct an experiment, incrementally build on experiments conducted by peers, and trace model lineage for compliance and audit verifications.

[SageMaker Feature Store](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store.html)

A centralized store for features and associated metadata so features can be easily discovered and reused. You can create two types of stores, an Online or Offline store. The Online Store can be used for low latency, real-time inference use cases and the Offline Store can be used for training and batch inference.

[SageMaker Ground Truth](https://docs.aws.amazon.com/sagemaker/latest/dg/sms.html)

To train a machine learning model, you need a large, high-quality, labeled dataset. Ground Truth helps you build high-quality training datasets for your machine learning models. With Ground Truth, you can use workers from either Amazon Mechanical Turk, a vendor company that you choose, or an internal, private workforce along with machine learning to enable you to create a labeled dataset. You can use the labeled dataset output from Ground Truth to train your own models. You can also use the output as a training dataset for an Amazon SageMaker AI model.

[SageMaker Ground Truth Plus](https://docs.aws.amazon.com/sagemaker/latest/dg/gtp.html)

A turnkey data labeling feature to create high-quality training datasets without having to build labeling applications and manage the labeling workforce on your own.

[SageMaker Inference Recommender](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender.html)

Get recommendations on inference instance types and configurations (e.g. instance count, container parameters and model optimizations) to use your ML models and workloads.

[Inference shadow tests](https://docs.aws.amazon.com/sagemaker/latest/dg/shadow-tests.html)

Evaluate any changes to your model-serving infrastructure by comparing its performance against the currently deployed infrastructure.

[SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html)

Learn about SageMaker AI features and capabilities through curated 1-click solutions, example notebooks, and pretrained models that you can deploy. You can also fine-tune the models and deploy them.

[SageMaker ML Lineage Tracking](https://docs.aws.amazon.com/sagemaker/latest/dg/lineage-tracking.html)

Track the lineage of machine learning workflows.

[SageMaker Model Building Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html)

Create and manage machine learning pipelines integrated directly with SageMaker AI jobs.

[SageMaker Model Cards](https://docs.aws.amazon.com/sagemaker/latest/dg/model-cards.html)

Document information about your ML models in a single place for streamlined governance and reporting throughout the ML lifecycle.

[SageMaker Model Dashboard](https://docs.aws.amazon.com/sagemaker/latest/dg/model-dashboard.html)

A pre-built, visual overview of all the models in your account. Model Dashboard integrates information from SageMaker Model Monitor, transform jobs, endpoints, lineage tracking, and CloudWatch so you can access high-level model information and track model performance in one unified view.

[SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html)

Monitor and analyze models in production (endpoints) to detect data drift and deviations in model quality.

[SageMaker Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)

Versioning, artifact and lineage tracking, approval workflow, and cross account support for deployment of your machine learning models.

[SageMaker Neo](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html)

Train machine learning models once, then run anywhere in the cloud and at the edge.

[Notebook-based Workflows](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-auto-run.html)

Run your SageMaker Studio notebook as a non-interactive, scheduled job.

[Preprocessing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)

Analyze and preprocess data, tackle feature engineering, and evaluate models.

[SageMaker Projects](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-projects.html)

Create end-to-end ML solutions with CI/CD by using SageMaker Projects.

[Reinforcement Learning](https://docs.aws.amazon.com/sagemaker/latest/dg/reinforcement-learning.html)

Maximize the long-term reward that an agent receives as a result of its actions.

[SageMaker Role Manager](https://docs.aws.amazon.com/sagemaker/latest/dg/role-manager.html)

Administrators can define least-privilege permissions for common ML activities using custom and preconfigured persona-based IAM roles.

[SageMaker Serverless Endpoints](https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html)

A serverless endpoint option for hosting your ML model. Automatically scales in capacity to serve your endpoint traffic. Removes the need to select instance types or manage scaling policies on an endpoint.

[Studio Classic Git extension](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-git-attach.html)

A Git extension to enter the URL of a Git repository, clone it into your environment, push changes, and view commit history.

[SageMaker Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks.html)

The next generation of SageMaker notebooks that include AWS IAM Identity Center (IAM Identity Center) integration, fast start-up times, and single-click sharing.

[SageMaker Studio Notebooks and Amazon EMR](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-notebooks-emr-cluster.html)

Easily discover, connect to, create, terminate and manage Amazon EMR clusters in single account and cross account configurations directly from SageMaker Studio.

[SageMaker Training Compiler](https://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler.html)

Train deep learning models faster on scalable GPU instances managed by SageMaker AI.<br>
