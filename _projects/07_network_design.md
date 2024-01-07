---
layout: page
title: Freight Network Design
description: A hub location problem in Turkey
img: assets/img/network_design/network_front.jpg
importance: 2
---

This is a personal project inspired by professor and friend Armin Lüer <https://scholar.google.com/citations?user=9F69pKwAAAAJ>.

Repository here: <https://github.com/fjzs/turkey_network>

## **1 Problem Definition**
Hubs location problems fall within the broad category of network design problems. A hub is tipically used passenger
transportaion, cargo industry, mail delivery (before e-mail was invented though), among others. The role of a hub is to
consolidate the flow from multiple sources to leverage economies of scale in the transportation.


## **2 Methodology**

## **3 Optimization Model**


### Sets
$$N$$: set of nodes


### Parameters
$$w_{ij}$$: flow from node $$i$$ to node $$j$$ (boxes)

$$s_i = \sum_{j \in N}w_{ij}$$: supply of node $$i$$ (boxes)

$$h_i$$: fixed cost of locating a hub at node $$i$$ ($)

$$d_{ij}$$: distance between nodes $$i$$ and $$j$$ (km)

$$v_t$$: velocity of trucks (km/h)

$$v_p$$: velocity of airplanes (km/h)

$$tt_{ij} = d_{ij} / v_t$$: travel time by truck between nodes $$i$$ and $$j$$ (h)

$$tp_{ij} = d_{ij} / v_p$$: travel time by plane between nodes $$i$$ and $$j$$ (h)

$$f_{ij}$$: fixed cost of using arc $$(i,j)$$ ($)

$$c$$: unit cost of transporting 1 box 1 km ($/box-km)

$$\alpha$$: cost factor of transfer activities between hubs (unitless)

$$\beta$$: maximum time delivery between any origin and destination (h)


### Variables
$$Z_{ik}$$: 1 if node $$i$$ is assigned to hub $$k$$ else 0, $$\forall i \in N, k \in N$$ (unitless)

$$F_{ik}$$: flow from node $$i$$ to hub $$k$$, $$\forall i \in N, k \in N$$ (boxes)

$$Y_{ikl}$$: flow from node $$i$$ going through hubs $$k$$ and $$l$$, $$\forall i \in N, k \in N, l \in K: k \neq l, i \neq l$$ (boxes)

$$X_{ilj}$$: flow from node $$i$$ going through hub $$l$$ to destination $$j$$, $$\forall i \in N, l \in N, j \in N: i \neq j$$ (boxes)

$$R_{kl}$$: 1 if hub $$k$$ is connected to hub $$l$$ else 0, $$\forall k \in N, l \in N: k \neq l$$ (unitless)

$$D^{h}_k$$: departure time from hub $$k$$ to transport cargo to other hubs, $$\forall k \in N$$ (h)

$$D^{c}_k$$: departure time from hub $$k$$ to transport cargo to customers, $$\forall k \in N$$ (h)

$$A_j$$: latest arrival time to destination $$j$$, $$\forall j \in N$$ (h)


### Objective Function
Location hubs cost:

$$\sum_{i \in N} Z_{ii} \cdot h_i$$

Links between nodes cost:

$$\sum_{i \in N}\sum_{k \in N} Z_{ik} \cdot f_{ik} + \sum_{k \in N}\sum_{l \in N} R_{kl} \cdot f_{kl} $$


Transportation cost (collection activities):

$$\sum_{i \in N}\sum_{k \in N} F_{ik} \cdot d_{ik} \cdot c$$

Transportation cost (transfer activities):

$$\sum_{i \in N}\sum_{k \in N}\sum_{l \in N} Y_{ikl} \cdot d_{kl}\cdot c \cdot \alpha$$

Transportation cost (distribution activities):

$$\sum_{i \in N}\sum_{l \in N}\sum_{j \in N} X_{ilj} \cdot d_{lj}\cdot c$$


### Constraints

(1) Assign every node to a single hub:

$$\sum_{k \in N}Z_{ik}= 1$$

$$\forall i \in N$$


(2) Assigning a node to a hub implies the hub is located:

$$Z_{ik} \leq Z_{kk}$$

$$\forall i \in N, k \in N: i \neq k$$


(3) The flow from each node goes fully to it's hub:

$$F_{ik} = Z_{ik} \cdot s_i$$

$$\forall i \in N, k \in N$$


(4) Flow from origin between hubs implies link between them:

$$Y_{ikl} \leq R_{kl} \cdot s_i$$

$$\forall i \in N, k \in N, l \in N: k \neq l, i \neq l$$


(5) Conservation of flow from origin $$i$$ at hub $$k$$:

$$F_{ik} + \sum_{h \in N}Y_{ihk} = \sum_{l \in N}Y_{ikl} + \sum_{j \in N}X_{ikj}$$

$$\forall i \in N, k \in N$$


(6) Flow from origin between hubs implies origin assigned to hub:

$$Y_{ikl} \leq Z_{ik} \cdot s_i$$

$$\forall i \in N, k \in N, l \in N: k \neq l, l \neq i$$

   
(7) Flow from origin to destination covered:

$$X_{ilj} = Z_{jl} \cdot w_{ij}$$

$$\forall i \in N, l \in N, j \in N: i \neq j$$
    

(8) Link between hubs implies hubs:

$$2R_{kl} \leq Z_{kk} + Z_{ll}$$

$$\forall k \in N, l \in N: k \neq l$$


(9) Plane departuring from hub $$k$$ to other hubs must wait for cargo from customers:

$$D^{h}_k \geq tt_{ik} \cdot Z_{ik}$$

$$\forall i \in N, k \in N: i \neq k$$
    

(10) Truck departure time from hub $$l$$ to its customers must wait for incoming air cargo from other hubs:

Original non-linear:

$$D^{c}_l \geq (D^{h}_k + tp_{kl}) \cdot R_{kl}$$

When linearized:

$$D^{c}_{l} \geq D^{h}_k + tp_{kl} - M(1 - R_{kl})$$

Where $$M$$ is a large enough number, in this case $$M = \beta$$.

$$\forall k \in N, l \in N$$


(11) Truck departure time from hub $$l$$ to its customer $$j$$ must wait from all customers to consolidate cargo (this is required for the case when a single hub is located to the network).

$$D^{c}_{l} \geq tt_{jl} \cdot Z_{jl}$$

$$\forall j \in N, l \in N$$


(12) Latest arrival to customer $$j$$ depends on the latest airplane arriving to its assigned hub $$l$$:

Original non-linear:

$$ A_j \geq (D^{c}_{l} + tt_{lj}) \cdot Z_{jl}$$

When linearized:

$$ A_j \geq D^{c}_{l} + tt_{lj} - M(1 - Z{jl})$$

Where $$M$$ is a large enough number, in this case $$M = \beta$$.

$$\forall j \in N, l \in N$$  
        

(13) Latest arrival to destination $$j$$ satisfies the service level:

$$A_j \leq \beta$$

$$\forall j \in N$$ 


### Toy example to visualize variables relations

Let's look at a little example of **5 nodes** to understand visually how they interact. We will take a look at 3 types of variables: hub decisions, flow decisions and timing decisions. In the figure below we start with the hub decisions.

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/network_design/example_hubs.png" 
        class = "img-fluid rounded z-depth-1"
        width = 700
        caption = "In this diagram you can see the network was organized with 2 hubs (nodes 3 and 4)."
    %}
</div></div>

Now we can move forward with flow decisions. Remember the flow is modeled as a multi-commodity problem, where each origin is a commodity. In the figure below you can see how the flow from each origin moves through the network.

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/network_design/example_flow.png" 
        class = "img-fluid rounded z-depth-1"
        width = 1500
        caption = "In this diagram you can see how each origin (I painted with red, blue and green the nodes the supply flow to the network) transports its flow. Recall that this is an uncapacitated problem, so there are no capacity constraints in the arcs of the graph."
    %}
</div></div>

Finally we can see the timing decisions in the next figure. From this example we can conclude that the fastest delivery time is 18 time units.

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/network_design/example_timing.png" 
        class = "img-fluid rounded z-depth-1"
        width = 1500
        caption = "In this diagram you can see how the timings are organized. Recall that the freight between non-hub nodes and hubs is supposed to be done in a cheap and slow mode (like trucks), and the freight between hubs in an expensive and fast mode (like airplanes). The number on each arc represent the time needed to traverse it by the defined transportation mode."
    %}
</div></div>
    

    