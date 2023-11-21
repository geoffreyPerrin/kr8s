# Listing resources

## List Nodes

Print out all of the node names in the cluster.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

for node in kr8s.get("nodes"):
    print(node.name)
```
````

````{tab-item} Async
```python
import kr8s.asyncio

for node in await kr8s.asyncio.get("nodes"):
    print(node.name)
```
````

`````

## List Pods in all Namespaces

List all Pods in all namespaces and print their IP, namespace and name.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

for pod in kr8s.get("pods", namespace=kr8s.ALL):
    print(pod.status.podIP, pod.metadata.namespace, pod.metadata.name)
```
````

````{tab-item} Async
```python
import kr8s

for pod in await kr8s.asyncio.get("pods", namespace=kr8s.ALL):
    print(pod.status.podIP, pod.metadata.namespace, pod.metadata.name)
```
````

`````

## List Ingresses (all styles)

List all Ingresses in the current namespace using all styles from shorthand to explicit group and version naming.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

# All of these are equivalent
ings = kr8s.get("ing")                           # Short name
ings = kr8s.get("ingress")                       # Singular
ings = kr8s.get("ingresses")                     # Plural
ings = kr8s.get("Ingress")                       # Title
ings = kr8s.get("ingress.networking.k8s.io")     # Full group name
ings = kr8s.get("ingress.v1.networking.k8s.io")  # Full with explicit version
ings = kr8s.get("ingress.networking.k8s.io/v1")  # Full with explicit version alt.
```
````

````{tab-item} Async
```python
import kr8s.asyncio

# All of these are equivalent
ings = await kr8s.asyncio.get("ing")                           # Short name
ings = await kr8s.asyncio.get("ingress")                       # Singular
ings = await kr8s.asyncio.get("ingresses")                     # Plural
ings = await kr8s.asyncio.get("Ingress")                       # Title
ings = await kr8s.asyncio.get("ingress.networking.k8s.io")     # Full group name
ings = await kr8s.asyncio.get("ingress.v1.networking.k8s.io")  # Full with explicit version
ings = await kr8s.asyncio.get("ingress.networking.k8s.io/v1")  # Full with explicit version alt.
```
````

`````
## List Ready Pods

Get a list of Pod resources that have the `Ready=True` condition.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

for pod in kr8s.get("pods", namespace="kube-system"):
    if pod.ready():
        print(pod.name)
```
````

````{tab-item} Async
```python
import kr8s

for pod in await kr8s.asyncio.get("pods", namespace="kube-system"):
    if await pod.ready():
        print(pod.name)
```
````

`````

## List Pods by label selector

Starting from a dictionary containing a label selector get all Pods from all Namespaces matching that label.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

selector = {'component': 'kube-scheduler'}

for pod in kr8s.get("pods", namespace=kr8s.ALL, label_selector=selector):
    print(pod.namespace, pod.name)
```
````

````{tab-item} Async
```python
import kr8s

selector = {'component': 'kube-scheduler'}

for pod in await kr8s.asyncio.get("pods", namespace=kr8s.ALL, label_selector=selector):
    print(pod.namespace, pod.name)
```
````

`````

## List Running Pods

Get a list of Pod resources that have `status.phase=Running` using a field selector.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

for pod in kr8s.get("pods", namespace="kube-system", field_selector="status.phase=Running"):
    print(pod.name)
```
````

````{tab-item} Async
```python
import kr8s

for pod in await kr8s.asyncio.get("pods", namespace="kube-system", field_selector="status.phase=Running"):
    print(pod.name)
```
````

`````

## List Pods sorted by restart count

List Pods and sort them by their restart count.

`````{tab-set}

````{tab-item} Sync
```python
import kr8s

pods = kr8s.get("pods", namespace=kr8s.ALL)
pods.sort(key=lambda pod: pod.status.containerStatuses[0].restartCount, reverse=True)

for pod in pods:
    print(pod.name, pod.status.containerStatuses[0].restartCount)
```
````

````{tab-item} Async
```python
import kr8s

pods = await kr8s.asyncio.get("pods", namespace=kr8s.ALL)
pods.sort(key=lambda pod: pod.status.containerStatuses[0].restartCount, reverse=True)

for pod in pods:
    print(pod.name, pod.status.containerStatuses[0].restartCount)
```
````

`````