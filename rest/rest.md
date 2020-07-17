# REST

Roy Fielding's dissertation is the authoritative source for REST (Representational State Transfer) 

REST is an architectural style for designing network-based application software.

It is an aggregation of various network based architectural styles, which, together, synergestically provide internet scale to any network based application.

##Five Network Styles

| Network Styles used by REST | Network Styles NOT used by REST |
| --- | --- |
| Client-Server | _Pipe Filter / Uniform Pipe Filter_ |
| Stateless requests between Client-Server | _Remote Session/Data Access/Evaluation_ |
| Layered systems between Client-Server (proxies, gateways, firewalls) | _Event Based Integration_ |
| COD (Code on Demand) | _(Brokered) Distributed Objects_ |
| Cache | |

Brief notes on these styles:

1. Client-Server:

    Separation of concerns. These are roles. Server provides services on request. Usually clients concern is UI.

1. Statelessness: 

    State per client request/session occupies space in memory and disk which are at a premium on servers. Prevents scaling. Using this style means that the interfaces are designed such that server never holds any data for future requests. Requests and responses therefore include all the information required.

1. Layered Systems
    - Multiple systems could be layered between client and server. An arrangement of systems in a unix pipe like fashion could lead to feature rich system.
    - Systems participating in data path help with security and many rich transformation features
    - Systems participating in routing help with scale (proxies and gateways)

1. Code-on-demand: 

    Download the 'know-how'; code and run on client. The 'know-how' could be understanding server responses, rendering them and even how to interact with server and get responses. An angular app is an example.

1. Cache

    Cache provides interface of a server and consumes interface of a server and provides caching in a transparent way. It can be part of layered systems or could be just a library at client or any layered systems.


Here is what an application gains by using REST.

| Metric | Benefit |
| --- | --- |
| User Perceived Performance | Cache provides improved average latency |
| Efficiency |Cache provides efficiency by avoiding duplicate work. <br/><br/>COD means that user interacts locally rather than through remote connections/sessions |
| Scalability | Client-Server style provides scalability by simplifying the server components by removing UI concernStatelessness <br/><br/> Not having to store state between requests allows the server component to quickly free resourcesLS <br/><br/>Intermediaries between client and server forming layers could be used for load balancing and thereby support more users<br/><br/>COD improves scaling since it can off-load work to the client that would otherwise have consumed its resources |
| Simplicity | Client-Server style provides simplicity because UI and Server are untangled<br/><br/>Due to statelessness, server doesn&#39;t have to manage resource usage across requests. |
| Extensibility/Configurability | COD provides ability to add features to a deployed client |
| Evolvability | Client-Server - Allows the UI and Servers to evolve independently<br/><br/>LS - Allows intermediaries to evolve independently.Uniform Interface - Implementations are decoupled from the services they provide |
| Visibility | Statelessness - All relevant data is in one place, i.e., request<br/><br/>Uniform Interface - Information is transferred in a standardized form, and messages are self-descriptive |
| Reliability | Statelessness eases the task of recovering from partial failures |
| Reusability | LS - with well defined incoming and outgoing, components become more portable and reusable.Allows reflective monitoring at all levels and improves failure management |
| Portability | LS - with well defined incoming and outgoing, components become more portable and reusable. |

There are some tradeoffs as well:

| Network Perf | Statelessness degraded due to repetitive data. |
| --- | --- |
| User Perceived Performance | LS - Intermediaries from client-server cause overhead due to multiple hops |
| Efficiency | Uniform interface degrades efficiency since information is transferred in a standardized form rather than one which is specific to an application&#39;s needs. |

###Note
So, although Roy Fielding [does not agree](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven) to the idea calling an API as restful, unless all constraints are applied, REST is not an all or nothing proposition. there are significant benefits to be gained from partial adoption.

## Uniform Interface

Uniform Interface (aka REST API) is one of the distinguishing style/constraint of REST. However, commonly, RESTfulness is incorrectly referred to this constraint alone.

REST is also mistakenly equated to features of HTTP. To understand uniform-interface, its best to proceed with notion that REST interfaces can be used over something other than HTTP, like say, ftp. Underlying protocol like HTTP is unimportant.

![](RackMultipart20200717-4-1ovh49_html_1c1126a5e758ea4b.png)
Fig: A generic component providing a generic interface

## &#39;Interfaces&#39;

The foremost thing is that the component interactions must be via interfaces, that is, via pre-defined contracts. For example, creation of an account on a bank involves sending an account opening request to a server which implements this contract.

A contract is composed of:

- Data Contract: (What data is required by the contract and what data is promised by the contract in response)
  - Pre-defined fields/values like types of accounts (&#39;Savings&#39;, &#39;Current&#39;), branch code etc
  - Mandatory (say, First name) and optional data (middle name)
  - Intended operation such as &#39;Open Account&#39;.
  - Allowed/Disallowed values - Only a certain set of countries could be allowed in residence address.
- Structure and format of the data being sent
  - JSON, XML, plan text etc
- Transport
  - Email, HTTP etc

The benefit is that a contract is durable and the implementor can evolve independently and change without causing the clients to change. (Evolvability).

[APIs are so ubiquitous what does it look like to not use interfaces? There is no such thing as communicating without an interface. Communication in whatever shape or form could be called as interface. Its just that the interface could be of poor quality. Maintaining even a poor quality interface does yield evolvability, its just that it would be harder to do]

## &#39;Uniform Interfaces&#39;

This means that _all_ interfaces (specifically the data contract part) provided by servers must be designed to adhere to a set of common semantics prescribed by REST.

These semantics are the same for all domains, industries etc. Just to drive this point, the semantics are the same for

- A service which provides customer support ticketing
- A service which makes hotel reservation
- A service which provides geo-maps
- A service which provides point to point delivery of goods
- So on and on.

## Why?

Why have these semantics?

My hypothesis is that there are 2 reasons.

Reason [1] - The interfaces must play well, synergistically with all the network styles of REST. If there were no semantic constraints, interfaces can be defined such that the benefits of using other styles are lost.

As a thought experiment, we could start with the semantics that each of the network styles require from interfaces.

1. Client-Server

A client does not have to change as long as the contract remains same.

1. Statelessness

Requests and responses must contain all relevant data.

1. Layered Systems

Intermediaries that help with data transformation and routing require visibility into

- Request and response data
- Nature of request - Idempotent? Safe?
- Metadata - media type, content length, user-agent etc.
- Destination address
- Destination service

Intermediaries involved in routing woud also need that the destination address and service do not change. Otherwise, a mechanism is required to dynamically change them whenever server changes them.

1. Cache

For requests/responses that are cacheable, a cache needs to be able to uniquely identify a request (all of it) and its variants.

Interface to be able to support cache control.

It would be an additional benefit for caches if the server does not modify any part of the request, especially the destination service and address.

1. COD

No specific requirement on interfaces.

1. Client - Server

No specific requirement on interfaces.

Reason [2]

Data Encapsulation

An interface may expose data model persisted in databases directly as part of the interface contracts. E.g, a user information API may provide Street1, Street2 attributes which are mere lines in a address.

Such internals must be hidden and the view provided to the users of the interface must be something like an &#39;address&#39;.

Reason [3]

While the above requirements ensure that the benefits listed in Table[1] are not lost, network performance, user perceivable performance and scalability can be improved if the number of interface interactions and their frequency are reduced.

Here are the interface semantics prescribed by REST (as per dissertation)

1. Identifiable resources. Use URL or URN to identify.
2. Manipulation of resources through representations;
3. Self-descriptive messages
4. Hypermedia as the engine of application state.

Here are the key excepts from dissertation on a resource, before we map semantics to the reasons.

&quot;The key abstraction of information in REST is a _resource_.&quot;

&quot;A resource is a conceptual mapping to a set of entities, not the entity that corresponds to the mapping at any particular point in time.&quot;

&quot;This abstract definition of a resource enables key features of the Web architecture. First, it provides generality by encompassing many sources of information without artificially distinguishing them by type or implementation. Second, it allows late binding of the reference to a representation, enabling content negotiation to take place based on characteristics of the request. Finally, it allows an author to reference the concept rather than some singular representation of that concept, thus removing the need to change all existing links whenever the representation changes (assuming the author used the right identifier).&quot;

--

Layered Systems and Statelessness necessitate &#39;self-descriptive messages&#39;. (dissertation itself mentions this)

A resource being an abstraction of something more detailed, larger and complex, while at the same time, comprehensible as a concept to API users (humans or agents) is very similar to a facade pattern. The pattern brings in conceptual conciseness, efficiency of communication and least coupling with it (Reason [3]).

Example-1 - Media player - lot of inner workings of media parsing, decoding, rendering, audio video sync is abstracted away with just a &#39;player&#39; supporting play, pause etc. Say, if the business is all about playback of media, a &#39;player&#39; is something more comprehensible to user and suitable. On the other hand, if the business is about media editing, &#39;mixer&#39; would be a suitable abstraction.

![](RackMultipart20200717-4-1ovh49_html_d54befec4a557189.png)

Identifiability of a resource at a global scale, is a generic need to reference and access resource. REST says that there has to be a uniform way to do this. That is, URN, URL or in general URI.

Cache and layered Systems support URI today and hypothetically, if required can only support a one or two more (again hypothetically). Companies and users cannot invent new ways of accessing resources. Without this uniformity, cache and layered systems dont impart their benefits. That is, this is necessitated by Reason [1].

------

Reason [2] and the nature of a resource in itself (being an abstract concept) necessitates representations.

But there is just one feature that is different. Multiple representations are possible for a single resource and client can negotiate with server on the version.

![](RackMultipart20200717-4-1ovh49_html_1a6874a3adaef820.png)

Hypermedia as the engine of application state (HATEOAS) is something that is derived from the success of hypertext web, described below.

A user visits a site and navigates as he/she chooses. A user always starts from home page, optioally logs in, navigates as per his/her needs to finish a task. The developers of the site are free to change the navigation structure of the site at will, without breaking anything and without causing any inconvenience to user.

This gives a high level of independent evolvability to the server. A server can

- Change URI structure
- Add/remove resources
- Remove relationships between resources

Intention of HATEOAS is to achieve the same success with REST. So it puts forward two constraints

1. URLs must be discovered by navigating through resources.
2. Clients must know how to invoke APIs and understand API responses, without having to depend on information shared out-of-band.

(1)

To see the benefit of (1) lets consider a service with 200 APIs. Lets consider that the organization which is providing this service has multiple tenants and has one client per tenant.

Below diagrams show how complex it would be to undertake a change in server, without resorting to API versioning and imbibing clients with intelligence to be compatible with the migration process.

![](RackMultipart20200717-4-1ovh49_html_16ef1ac2ca26f283.png)

Compare this with HATEOAS:

![](RackMultipart20200717-4-1ovh49_html_ee315ffc8485f0bc.png)

An additional indirection gives return on investment in the evolution of products and platforms, even though it seems overly complex to fetch just one URL.

(2)

This is a harder problem. The ultimate goal is that the client will almost never have to change to adapt to responses, just like browser.

Lets compare the hypertext scenario with REST scenario.

Hypertext

A format such as text/html is well defined and clients like browser are built with the understanding of the specification. They know how to process a text/html document.

With this preparedness,

- A client (browser) retrieves a hypertext and understands how to render.
- When a user clicks on an anchor, it knows that it has to perform a GET
  - (When the user clicks on an image, it knows that it has to do nothing.)
- When a user clicks on submit of a form, it knows that it has to do a POST with data thats filled in.

REST

Service owner defines a media type which partially or fully defines the structure of all representations and meaning of various fields ([common vocabulary](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)). This media type definition is available to clients publicly.

A media type may not be universally usable (Atom&#39;s media type is an exception). It may be atleast usable within an entire company, and at most be reusable within the same domain/industry.

For example, notions of a bank account, cheque, demand drafts could all be distilled into a certain &#39;media type&#39;.

if such media type exists, that is common across banks, various developers may create client applications that can work with various banks without having to change when banks change something at their end.

It may not be as well specified as html.

With the understanding of &#39;media type&#39;, a client can be built, such that it

- It navigates to a desired resource using known relationships between resources
  - At every step of navigation, a client may or may not be directed by a human with choices
- It performs a desired action on the resource.

For example, a bank client app can

- Be programmed to ask for user credentials, login using one of the initially discovered links.
- Be programmed to display a dashboard. For this task, it looks for
  - Links with relationships as &#39;accounts&#39;
  - Links with relationships as &#39;creditcards&#39;
  - Links with relationships as &#39;mutualfunds&#39;
  - Etc
  - This exact list of relationships could also be retrieved from another dashboard meta API.
  - By changing the dashboard meta api response, the contents of dashboard could also be changed.
  - Or, it displays a list where each item corresponds to a relationship.
- Be programmed to take action on a resource
  - On navigation to credit card resource, a client could provide option to hotlist a card. As part of media type definition, clients must know that &#39;hotlist&#39; is an action and client must POST. On confirmation from user, client invokes the API.

