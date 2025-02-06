# Inference methods

Amazon SageMaker offers a variety of inference options to suit different machine learning deployment needs\[1]\[2]. These options include:

* **Real-Time Inference:** Provides predictions with immediate, low-latency responses, suitable for applications requiring instant feedback such as fraud detection, recommendation systems, and chatbots\[5]\[6].
* **Serverless Inference:** Enables easy scaling to handle thousands of models per endpoint and millions of transactions per second, with sub-10 millisecond overhead latencies\[1].
* **Asynchronous Inference:** Allows users to make requests for predictions and receive the results later, which is useful for applications that can afford to wait for the results\[6]. It is also cost-effective due to the efficient utilization of computing resources\[5].
* **Batch Transform:** An offline inferencing option for when you have very large datasets\[2]. SageMaker Processing offers a managed compute environment to run a custom batch inference container with a custom script\[7].

Amazon SageMaker also provides options for optimizing inference performance and cost, such as deploying multiple models on a single endpoint, using serial inference pipelines, and autoscaling compute resources\[1]. You can also shadow test models to validate their performance and use intelligent routing to reduce latency\[1].
