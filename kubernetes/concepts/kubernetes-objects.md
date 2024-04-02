# Kubernetes Objects

_Kubernetes objects_ are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster.

A Kubernetes object is a "<mark style="background-color:purple;">record of intent</mark>"--<mark style="color:purple;">once you create the object, the Kubernetes system will constantly work to ensure that object exists.</mark>

To work with Kubernetes objects—whether to create, modify, or delete them—you'll need to use the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/).&#x20;

Almost every Kubernetes object includes two nested object fields that govern the object's configuration:&#x20;

* the object _`spec`_ and&#x20;
* the object _`status`_. _<mark style="color:purple;">current state</mark>_ <mark style="color:purple;"></mark><mark style="color:purple;">of the object,</mark>

## Object structure



In the manifest (YAML or JSON file) for the Kubernetes object you want to create, you'll need to set values for the following fields:

* `apiVersion` - Which version of the Kubernetes API you're using to create this object
* `kind` - What kind of object you want to create
* `metadata` - Data that helps uniquely identify the object, including a `name` string, `UID`, and optional `namespace`
* `spec` - What state you desire for the object
  * The precise format of the object `spec` is different for every Kubernetes object, and contains nested fields specific to that object.
