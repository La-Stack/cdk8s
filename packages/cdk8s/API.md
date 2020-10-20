# API Reference

**Classes**

Name|Description
----|-----------
[ApiObject](#cdk8s-apiobject)|*No description*
[App](#cdk8s-app)|Represents a cdk8s application.
[Chart](#cdk8s-chart)|*No description*
[DependencyGraph](#cdk8s-dependencygraph)|Represents the dependency graph for a given Node.
[DependencyVertex](#cdk8s-dependencyvertex)|Represents a vertex in the graph.
[Helm](#cdk8s-helm)|Represents a Helm deployment.
[Include](#cdk8s-include)|Reads a YAML manifest from a file or a URL and defines all resources as API objects within the defined scope.
[Lazy](#cdk8s-lazy)|*No description*
[Metadata](#cdk8s-metadata)|Scope-wide k8s metadata.
[Names](#cdk8s-names)|Utilities for generating unique and stable names.
[Testing](#cdk8s-testing)|Testing utilities for cdk8s applications.
[Yaml](#cdk8s-yaml)|YAML utilities.


**Structs**

Name|Description
----|-----------
[ApiObjectMetadata](#cdk8s-apiobjectmetadata)|Metadata associated with this object.
[ApiObjectOptions](#cdk8s-apiobjectoptions)|Options for defining API objects.
[AppOptions](#cdk8s-appoptions)|*No description*
[ChartOptions](#cdk8s-chartoptions)|*No description*
[HelmOptions](#cdk8s-helmoptions)|Options for `Helm`.
[IncludeOptions](#cdk8s-includeoptions)|*No description*


**Interfaces**

Name|Description
----|-----------
[IAnyProducer](#cdk8s-ianyproducer)|*No description*



## class ApiObject 🔹 <a id="cdk8s-apiobject"></a>



__Implements__: [IConstruct](#constructs-iconstruct)
__Extends__: [Construct](#constructs-construct)

### Initializer


Defines an API object.

```ts
new ApiObject(scope: Construct, ns: string, options: ApiObjectOptions)
```

* **scope** (<code>[Construct](#constructs-construct)</code>)  the construct scope.
* **ns** (<code>string</code>)  namespace.
* **options** (<code>[ApiObjectOptions](#cdk8s-apiobjectoptions)</code>)  options.
  * **apiVersion** (<code>string</code>)  API version. 
  * **kind** (<code>string</code>)  Resource kind. 
  * **metadata** (<code>[ApiObjectMetadata](#cdk8s-apiobjectmetadata)</code>)  Object metadata. __*Optional*__



### Properties


Name | Type | Description 
-----|------|-------------
**apiGroup**🔹 | <code>string</code> | The group portion of the API version (e.g. `authorization.k8s.io`).
**apiVersion**🔹 | <code>string</code> | The object's API version (e.g. `authorization.k8s.io/v1`).
**chart**🔹 | <code>[Chart](#cdk8s-chart)</code> | The chart in which this object is defined.
**kind**🔹 | <code>string</code> | The object kind.
**metadata**🔹 | <code>[Metadata](#cdk8s-metadata)</code> | Metadata associated with this API object.
**name**🔹 | <code>string</code> | The name of the API object.

### Methods


#### addDependency(...dependencies)🔹 <a id="cdk8s-apiobject-adddependency"></a>

Create a dependency between this ApiObject and other constructs.

These can be other ApiObjects, Charts, or custom.

```ts
addDependency(...dependencies: IConstruct[]): void
```

* **dependencies** (<code>[IConstruct](#constructs-iconstruct)</code>)  the dependencies to add.




#### toJson()🔹 <a id="cdk8s-apiobject-tojson"></a>

Renders the object to Kubernetes JSON.

```ts
toJson(): any
```


__Returns__:
* <code>any</code>



## class App 🔹 <a id="cdk8s-app"></a>

Represents a cdk8s application.

__Implements__: [IConstruct](#constructs-iconstruct)
__Extends__: [Construct](#constructs-construct)

### Initializer


Defines an app.

```ts
new App(options?: AppOptions)
```

* **options** (<code>[AppOptions](#cdk8s-appoptions)</code>)  configuration options.
  * **outdir** (<code>string</code>)  The directory to output Kubernetes manifests. __*Default*__: CDK8S_OUTDIR if defined, otherwise "dist"



### Properties


Name | Type | Description 
-----|------|-------------
**outdir**🔹 | <code>string</code> | The output directory into which manifests will be synthesized.

### Methods


#### synth()🔹 <a id="cdk8s-app-synth"></a>

Synthesizes all manifests to the output directory.

```ts
synth(): void
```







## class Chart 🔹 <a id="cdk8s-chart"></a>



__Implements__: [IConstruct](#constructs-iconstruct)
__Extends__: [Construct](#constructs-construct)

### Initializer




```ts
new Chart(scope: Construct, ns: string, options?: ChartOptions)
```

* **scope** (<code>[Construct](#constructs-construct)</code>)  *No description*
* **ns** (<code>string</code>)  *No description*
* **options** (<code>[ChartOptions](#cdk8s-chartoptions)</code>)  *No description*
  * **annotations** (<code>Map<string, string></code>)  Chart-scope annotations which apply to all API objects within this chart. __*Default*__: no chart-scope labels.
  * **labels** (<code>Map<string, string></code>)  Chart-scope labels which apply to all API objects within this chart. __*Default*__: no chart-scope labels.
  * **namespace** (<code>string</code>)  The default namespace for all objects defined in this chart (directly or indirectly). __*Default*__: no namespace is synthesized (usually this implies "default")



### Properties


Name | Type | Description 
-----|------|-------------
**metadata**🔹 | <code>[Metadata](#cdk8s-metadata)</code> | Chart-scope metadata.
**namespace**?🔹 | <code>string</code> | The default namespace for all objects in this chart.<br/>__*Optional*__

### Methods


#### addDependency(...dependencies)🔹 <a id="cdk8s-chart-adddependency"></a>

Create a dependency between this Chart and other constructs.

These can be other ApiObjects, Charts, or custom.

```ts
addDependency(...dependencies: IConstruct[]): void
```

* **dependencies** (<code>[IConstruct](#constructs-iconstruct)</code>)  the dependencies to add.




#### generateObjectName(apiObject)🔹 <a id="cdk8s-chart-generateobjectname"></a>

Generates a app-unique name for an object given it's construct node path.

Different resource types may have different constraints on names
(`metadata.name`). The previous version of the name generator was
compatible with DNS_SUBDOMAIN but not with DNS_LABEL.

For example, `Deployment` names must comply with DNS_SUBDOMAIN while
`Service` names must comply with DNS_LABEL.

Since there is no formal specification for this, the default name
generation scheme for kubernetes objects in cdk8s was changed to DNS_LABEL,
since it’s the common denominator for all kubernetes resources
(supposedly).

You can override this method if you wish to customize object names at the
chart level.

```ts
generateObjectName(apiObject: ApiObject): string
```

* **apiObject** (<code>[ApiObject](#cdk8s-apiobject)</code>)  The API object to generate a name for.

__Returns__:
* <code>string</code>

#### toJson()🔹 <a id="cdk8s-chart-tojson"></a>

Renders this chart to a set of Kubernetes JSON resources.

```ts
toJson(): Array<any>
```


__Returns__:
* <code>Array<any></code>

#### *static* of(c)🔹 <a id="cdk8s-chart-of"></a>

Finds the chart in which a node is defined.

```ts
static of(c: IConstruct): Chart
```

* **c** (<code>[IConstruct](#constructs-iconstruct)</code>)  a construct node.

__Returns__:
* <code>[Chart](#cdk8s-chart)</code>



## class DependencyGraph 🔹 <a id="cdk8s-dependencygraph"></a>

Represents the dependency graph for a given Node.

This graph includes the dependency relationships between all nodes in the
node (construct) sub-tree who's root is this Node.

Note that this means that lonely nodes (no dependencies and no dependants) are also included in this graph as
childless children of the root node of the graph.

The graph does not include cross-scope dependencies. That is, if a child on the current scope depends on a node
from a different scope, that relationship is not represented in this graph.


### Initializer




```ts
new DependencyGraph(node: Node)
```

* **node** (<code>[Node](#constructs-node)</code>)  *No description*



### Properties


Name | Type | Description 
-----|------|-------------
**root**🔹 | <code>[DependencyVertex](#cdk8s-dependencyvertex)</code> | Returns the root of the graph.

### Methods


#### topology()🔹 <a id="cdk8s-dependencygraph-topology"></a>



```ts
topology(): Array<IConstruct>
```


__Returns__:
* <code>Array<[IConstruct](#constructs-iconstruct)></code>



## class DependencyVertex 🔹 <a id="cdk8s-dependencyvertex"></a>

Represents a vertex in the graph.

The value of each vertex is an `IConstruct` that is accessible via the `.value` getter.


### Initializer




```ts
new DependencyVertex(value?: IConstruct)
```

* **value** (<code>[IConstruct](#constructs-iconstruct)</code>)  *No description*



### Properties


Name | Type | Description 
-----|------|-------------
**inbound**🔹 | <code>Array<[DependencyVertex](#cdk8s-dependencyvertex)></code> | Returns the parents of the vertex (i.e dependants).
**outbound**🔹 | <code>Array<[DependencyVertex](#cdk8s-dependencyvertex)></code> | Returns the children of the vertex (i.e dependencies).
**value**?🔹 | <code>[IConstruct](#constructs-iconstruct)</code> | Returns the IConstruct this graph vertex represents.<br/>__*Optional*__

### Methods


#### addChild(dep)🔹 <a id="cdk8s-dependencyvertex-addchild"></a>

Adds a vertex as a dependency of the current node.

Also updates the parents of `dep`, so that it contains this node as a parent.

This operation will fail in case it creates a cycle in the graph.

```ts
addChild(dep: DependencyVertex): void
```

* **dep** (<code>[DependencyVertex](#cdk8s-dependencyvertex)</code>)  The dependency.




#### topology()🔹 <a id="cdk8s-dependencyvertex-topology"></a>

Returns a topologically sorted array of the constructs in the sub-graph.

```ts
topology(): Array<IConstruct>
```


__Returns__:
* <code>Array<[IConstruct](#constructs-iconstruct)></code>



## class Helm 🔹 <a id="cdk8s-helm"></a>

Represents a Helm deployment.

Use this construct to import an existing Helm chart and incorporate it into your constructs.

__Implements__: [IConstruct](#constructs-iconstruct)
__Extends__: [Include](#cdk8s-include)

### Initializer




```ts
new Helm(scope: Construct, id: string, opts: HelmOptions)
```

* **scope** (<code>[Construct](#constructs-construct)</code>)  *No description*
* **id** (<code>string</code>)  *No description*
* **opts** (<code>[HelmOptions](#cdk8s-helmoptions)</code>)  *No description*
  * **chart** (<code>string</code>)  The chart name to use. It can be a chart from a helm repository or a local directory. 
  * **helmExecutable** (<code>string</code>)  The local helm executable to use in order to create the manifest the chart. __*Default*__: "helm"
  * **helmFlags** (<code>Array<string></code>)  Additional flags to add to the `helm` execution. __*Default*__: []
  * **releaseName** (<code>string</code>)  The release name. __*Default*__: if unspecified, a name will be allocated based on the construct path
  * **values** (<code>Map<string, any></code>)  Values to pass to the chart. __*Default*__: If no values are specified, chart will use the defaults.



### Properties


Name | Type | Description 
-----|------|-------------
**releaseName**🔹 | <code>string</code> | The helm release name.



## class Include 🔹 <a id="cdk8s-include"></a>

Reads a YAML manifest from a file or a URL and defines all resources as API objects within the defined scope.

The names (`metadata.name`) of imported resources will be preserved as-is
from the manifest.

__Implements__: [IConstruct](#constructs-iconstruct)
__Extends__: [Construct](#constructs-construct)

### Initializer




```ts
new Include(scope: Construct, name: string, options: IncludeOptions)
```

* **scope** (<code>[Construct](#constructs-construct)</code>)  *No description*
* **name** (<code>string</code>)  *No description*
* **options** (<code>[IncludeOptions](#cdk8s-includeoptions)</code>)  *No description*
  * **url** (<code>string</code>)  Local file path or URL which includes a Kubernetes YAML manifest. 



### Properties


Name | Type | Description 
-----|------|-------------
**apiObjects**🔹 | <code>Array<[ApiObject](#cdk8s-apiobject)></code> | Returns all the included API objects.



## class Lazy 🔹 <a id="cdk8s-lazy"></a>




### Methods


#### produce()🔹 <a id="cdk8s-lazy-produce"></a>



```ts
produce(): any
```


__Returns__:
* <code>any</code>

#### *static* any(producer)🔹 <a id="cdk8s-lazy-any"></a>



```ts
static any(producer: IAnyProducer): any
```

* **producer** (<code>[IAnyProducer](#cdk8s-ianyproducer)</code>)  *No description*

__Returns__:
* <code>any</code>



## class Metadata 🔹 <a id="cdk8s-metadata"></a>

Scope-wide k8s metadata.

Metadata is applied top-down. This means that API-object-level metadata will
override any metadata specified by it's parent, etc.

__Implements__: [IConstruct](#constructs-iconstruct)
__Extends__: [Construct](#constructs-construct)

### Methods


#### add(key, value)🔹 <a id="cdk8s-metadata-add"></a>

Adds an arbitrary key/value to the object metadata.

```ts
add(key: string, value: any): void
```

* **key** (<code>string</code>)  Metadata key.
* **value** (<code>any</code>)  Metadata value.




#### addAnnotation(name, value?)🔹 <a id="cdk8s-metadata-addannotation"></a>

Adds an annotation to all API objects within this scope.

```ts
addAnnotation(name: string, value?: string): void
```

* **name** (<code>string</code>)  The annotation.
* **value** (<code>string</code>)  Annotation value.




#### addAnnotations(annotations)🔹 <a id="cdk8s-metadata-addannotations"></a>

Adds multiple annotations to this scope.

```ts
addAnnotations(annotations: Map<string, string>): void
```

* **annotations** (<code>Map<string, string></code>)  Annotations to add.




#### addLabel(name, value?)🔹 <a id="cdk8s-metadata-addlabel"></a>

Adds a labels to all the API objects within a scope.

These labels will be added to all API objects within this scope unless the
same label is set within a narrower scope or at the API object itself.

If the value is set to `undefined` the label is removed from the scope &
all API objects (equivalent to `removeLabel`).

```ts
addLabel(name: string, value?: string): void
```

* **name** (<code>string</code>)  The name of the label.
* **value** (<code>string</code>)  The value of the label, or `undefined` to remove the label from the scope.




#### addLabels(labels)🔹 <a id="cdk8s-metadata-addlabels"></a>

Adds multiple labels to this scope.

```ts
addLabels(labels: Map<string, string>): void
```

* **labels** (<code>Map<string, string></code>)  The labels to add.




#### addNamespace(value?)🔹 <a id="cdk8s-metadata-addnamespace"></a>

Sets the namespace for this scope.

All API objects within this scope will use this scope will use this
namespace unless a namespace is defined within a narrower scope (or at the
API object itself).

```ts
addNamespace(value?: string): void
```

* **value** (<code>string</code>)  *No description*




#### clearNamespace()🔹 <a id="cdk8s-metadata-clearnamespace"></a>

Clears the namespace definition from the scope and all API objects within this scope.

```ts
clearNamespace(): void
```





#### removeAnnotation(name)🔹 <a id="cdk8s-metadata-removeannotation"></a>

Removes an annotation from all API objects within the scope.

```ts
removeAnnotation(name: string): void
```

* **name** (<code>string</code>)  The annotation to remove.




#### removeLabel(label)🔹 <a id="cdk8s-metadata-removelabel"></a>

Removes a label from all API objects within the scope.

```ts
removeLabel(label: string): void
```

* **label** (<code>string</code>)  The label to remove.




#### *static* of(scope)🔹 <a id="cdk8s-metadata-of"></a>

Returns a `Metadata` object associated with a specific scope.

```ts
static of(scope: IConstruct): Metadata
```

* **scope** (<code>[IConstruct](#constructs-iconstruct)</code>)  The construct scope.

__Returns__:
* <code>[Metadata](#cdk8s-metadata)</code>

#### *static* resolve(scope)🔹 <a id="cdk8s-metadata-resolve"></a>

Returns the resolved metadata for a scope by looking up metadata defined for this scope or parent scopes.

```ts
static resolve(scope: IConstruct): ApiObjectMetadata
```

* **scope** (<code>[IConstruct](#constructs-iconstruct)</code>)  The scope to resolve for.

__Returns__:
* <code>[ApiObjectMetadata](#cdk8s-apiobjectmetadata)</code>



## class Names 🔹 <a id="cdk8s-names"></a>

Utilities for generating unique and stable names.


### Methods


#### *static* toDnsLabel(path, maxLen?)🔹 <a id="cdk8s-names-todnslabel"></a>

Generates a unique and stable name compatible DNS_LABEL from RFC-1123 from a path.

The generated name will:
  - contain at most 63 characters
  - contain only lowercase alphanumeric characters or ‘-’
  - start with an alphanumeric character
  - end with an alphanumeric character

The generated name will have the form:
  <comp0>-<comp1>-..-<compN>-<short-hash>

Where <comp> are the path components (assuming they are is separated by
"/").

Note that if the total length is longer than 63 characters, we will trim
the first components since the last components usually encode more meaning.

```ts
static toDnsLabel(path: string, maxLen?: number): string
```

* **path** (<code>string</code>)  a path to a node (components separated by "/").
* **maxLen** (<code>number</code>)  maximum allowed length for name.

__Returns__:
* <code>string</code>

#### *static* toLabelValue(path, delim?, maxLen?)🔹 <a id="cdk8s-names-tolabelvalue"></a>

Generates a unique and stable name compatible label key name segment and label value from a path.

The name segment is required and must be 63 characters or less, beginning
and ending with an alphanumeric character ([a-z0-9A-Z]) with dashes (-),
underscores (_), dots (.), and alphanumerics between.

Valid label values must be 63 characters or less and must be empty or
begin and end with an alphanumeric character ([a-z0-9A-Z]) with dashes
(-), underscores (_), dots (.), and alphanumerics between.

The generated name will have the form:
  <comp0><delim><comp1><delim>..<delim><compN><delim><short-hash>

Where <comp> are the path components (assuming they are is separated by
"/").

Note that if the total length is longer than 63 characters, we will trim
the first components since the last components usually encode more meaning.

```ts
static toLabelValue(path: string, delim?: string, maxLen?: number): string
```

* **path** (<code>string</code>)  a path to a node (components separated by "/").
* **delim** (<code>string</code>)  a delimiter to separates components.
* **maxLen** (<code>number</code>)  maximum allowed length for name.

__Returns__:
* <code>string</code>



## class Testing 🔹 <a id="cdk8s-testing"></a>

Testing utilities for cdk8s applications.


### Methods


#### *static* app()🔹 <a id="cdk8s-testing-app"></a>

Returns an app for testing with the following properties: - Output directory is a temp dir.

```ts
static app(): App
```


__Returns__:
* <code>[App](#cdk8s-app)</code>

#### *static* chart()🔹 <a id="cdk8s-testing-chart"></a>



```ts
static chart(): Chart
```


__Returns__:
* <code>[Chart](#cdk8s-chart)</code>

#### *static* synth(chart)🔹 <a id="cdk8s-testing-synth"></a>

Returns the Kubernetes manifest synthesized from this chart.

```ts
static synth(chart: Chart): Array<any>
```

* **chart** (<code>[Chart](#cdk8s-chart)</code>)  *No description*

__Returns__:
* <code>Array<any></code>



## class Yaml 🔹 <a id="cdk8s-yaml"></a>

YAML utilities.


### Methods


#### *static* load(urlOrFile)🔹 <a id="cdk8s-yaml-load"></a>

Downloads a set of YAML documents (k8s manifest for example) from a URL or a file and returns them as javascript objects.

Empty documents are filtered out.

```ts
static load(urlOrFile: string): Array<any>
```

* **urlOrFile** (<code>string</code>)  a URL of a file path to load from.

__Returns__:
* <code>Array<any></code>

#### *static* save(filePath, docs)🔹 <a id="cdk8s-yaml-save"></a>

Saves a set of objects as a multi-document YAML file.

```ts
static save(filePath: string, docs: Array<any>): void
```

* **filePath** (<code>string</code>)  The output path.
* **docs** (<code>Array<any></code>)  The set of objects.




#### *static* tmp(docs)🔹 <a id="cdk8s-yaml-tmp"></a>

Saves a set of YAML documents into a temp file (in /tmp).

```ts
static tmp(docs: Array<any>): string
```

* **docs** (<code>Array<any></code>)  the set of documents to save.

__Returns__:
* <code>string</code>



## struct ApiObjectMetadata 🔹 <a id="cdk8s-apiobjectmetadata"></a>

__Obtainable from__: [Metadata](#cdk8s-metadata).[resolve](#cdk8s-metadata#cdk8s-metadata-resolve)()

Metadata associated with this object.



Name | Type | Description 
-----|------|-------------
**annotations**?🔹 | <code>Map<string, string></code> | Annotations is an unstructured key value map stored with a resource that may be set by external tools to store and retrieve arbitrary metadata.<br/>__*Default*__: No annotations.
**labels**?🔹 | <code>Map<string, string></code> | Map of string keys and values that can be used to organize and categorize (scope and select) objects.<br/>__*Default*__: No labels.
**name**?🔹 | <code>string</code> | The unique, namespace-global, name of this object inside the Kubernetes cluster.<br/>__*Default*__: an app-unique name generated by the chart
**namespace**?🔹 | <code>string</code> | Namespace defines the space within each name must be unique.<br/>__*Default*__: undefined (will be assigned to the 'default' namespace)



## struct ApiObjectOptions 🔹 <a id="cdk8s-apiobjectoptions"></a>


Options for defining API objects.



Name | Type | Description 
-----|------|-------------
**apiVersion**🔹 | <code>string</code> | API version.
**kind**🔹 | <code>string</code> | Resource kind.
**metadata**?🔹 | <code>[ApiObjectMetadata](#cdk8s-apiobjectmetadata)</code> | Object metadata.<br/>__*Optional*__



## struct AppOptions 🔹 <a id="cdk8s-appoptions"></a>






Name | Type | Description 
-----|------|-------------
**outdir**?🔹 | <code>string</code> | The directory to output Kubernetes manifests.<br/>__*Default*__: CDK8S_OUTDIR if defined, otherwise "dist"



## struct ChartOptions 🔹 <a id="cdk8s-chartoptions"></a>






Name | Type | Description 
-----|------|-------------
**annotations**?🔹 | <code>Map<string, string></code> | Chart-scope annotations which apply to all API objects within this chart.<br/>__*Default*__: no chart-scope labels.
**labels**?🔹 | <code>Map<string, string></code> | Chart-scope labels which apply to all API objects within this chart.<br/>__*Default*__: no chart-scope labels.
**namespace**?🔹 | <code>string</code> | The default namespace for all objects defined in this chart (directly or indirectly).<br/>__*Default*__: no namespace is synthesized (usually this implies "default")



## struct HelmOptions 🔹 <a id="cdk8s-helmoptions"></a>


Options for `Helm`.



Name | Type | Description 
-----|------|-------------
**chart**🔹 | <code>string</code> | The chart name to use. It can be a chart from a helm repository or a local directory.
**helmExecutable**?🔹 | <code>string</code> | The local helm executable to use in order to create the manifest the chart.<br/>__*Default*__: "helm"
**helmFlags**?🔹 | <code>Array<string></code> | Additional flags to add to the `helm` execution.<br/>__*Default*__: []
**releaseName**?🔹 | <code>string</code> | The release name.<br/>__*Default*__: if unspecified, a name will be allocated based on the construct path
**values**?🔹 | <code>Map<string, any></code> | Values to pass to the chart.<br/>__*Default*__: If no values are specified, chart will use the defaults.



## interface IAnyProducer 🔹 <a id="cdk8s-ianyproducer"></a>



### Methods


#### produce()🔹 <a id="cdk8s-ianyproducer-produce"></a>



```ts
produce(): any
```


__Returns__:
* <code>any</code>



## struct IncludeOptions 🔹 <a id="cdk8s-includeoptions"></a>






Name | Type | Description 
-----|------|-------------
**url**🔹 | <code>string</code> | Local file path or URL which includes a Kubernetes YAML manifest.


