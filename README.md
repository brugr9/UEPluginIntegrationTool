# Unreal&reg; Engine Plugin: Integration Tool &mdash; Readme

This document is part of *Unreal&reg; Engine Plugin: Integration Tool &ndash; Documentation*

* Author: Copyright 2022 Roland Bruggmann aka brugr9
* Profile on UE Marketplace: [https://www.unrealengine.com/marketplace/profile/brugr9](https://www.unrealengine.com/marketplace/profile/brugr9)
* Profile on Epic Developer Community: [https://dev.epicgames.com/community/profile/PQBq/brugr9](https://dev.epicgames.com/community/profile/PQBq/brugr9)

---
<!-- UE Marketplace : Begin 1/2 -->

![FeaturedImage](Docs/FeaturedImage894x488.jpg "FeaturedImage")

Adds Blueprint Support for Asynchronous Messaging using *NNG&trade; next generation of nanomsg&trade;* Software

## Description

This plugin enables asynchronous, broker-less messaging using *NNG&trade; next generation of nanomsg&trade;* software from the Blueprint Visual Scripting system.

The delivered assets provide transporting messages over a network and can be used in games to enable direct machine to machine communication, internet of things integration, or interaction with an event broker. Other use cases could be data streaming or instant messaging from or into a game.

Suits well for the use with, e.g., [NanoMQ&trade;](https://nanomq.io/) which may act as a nanomsg/NNG proxy providing with MQTT and ZeroMQ protocol.

<!-- UE Marketplace : End 1/2 -->
---

<div style='page-break-after: always'></div>

## Table of Contents

<!-- Start Document Outline -->

* [1. Installation](#1-installation)
* [2. Usage](#2-usage)
  * [2.1. Concept](#21-concept)
  * [2.2. Actors](#22-actors)
    * [2.2.1. PUB-Socket Actor](#221-pub-socket-actor)
    * [2.2.2. SUB-Socket Actor](#222-sub-socket-actor)
  * [2.3. Actor-Components](#23-actor-components)
    * [2.3.1. Publisher Actor-Component](#231-publisher-actor-component)
    * [2.3.2. Subscriber Actor-Component](#232-subscriber-actor-component)
* [3. Demo](#3-demo)
* [4. Unsupported](#4-unsupported)
* [A. Attribution](#a-attribution)
* [B. References](#b-references)
* [C. Citation](#c-citation)

<!-- End Document Outline -->

## 1. Installation

Startup the Unreal&reg; editor, and from the menu 'Edit > Plugins' access the 'Plugin Editor'. In the 'Plugin Editor', under category 'Messaging' find and enable the plugin.

![Screenshot of Plugin Editor with Plugin Integration Tool](Docs/ScreenshotPlugin.jpg "Screenshot of Plugin Editor with Plugin Integration Tool")
<br>*Fig. 1.: Screenshot of Plugin Editor with Plugin 'Integration Tool'*

Finally restart the editor. When the plugin has been loaded successfully, the output log displays a message with the custom log category LogNextGenMsg informing about the library version used:

```log
LogNextGenMsg: Using NNG version 1.5.2
```

<div style='page-break-after: always'></div>

## 2. Usage

### 2.1. Concept

Publisher and subscriber as actor components are responsible for publishing messages and subscribing to topics and receiving messages. These components contain as a variable a topic to publish or subscribe to. As another variable they contain a reference to a PUB- or SUB-socket instance they work with.

* One or multiple Publisher Actor-Components can access the same PUB-Socket Actor to send messages to the same endpoint (`Publisher : PUB-Socket = n : 1`).
* One or multiple Subscriber Actor-Components can access the same SUB-Socket Actor to subscribe to a topic and to receive messages from the same endpoint (`Subscriber : SUB-Socket = n : 1`).

### 2.2. Actors

The plugin provides with PUB-Sockets and SUB-Sockets which are Actors and may be found in the 'Place Actors' panel, category 'All Classes' and can be added to a map by drag'n'drop. The added actors then are listed in the world outliner. Please consider that in the viewport no sprites are shown for these sockets.

![Screenshot of Editor Tab 'Place Actors' listing Socket Actors](Docs/ScreenshotActors.jpg "Screenshot of Editor Tab 'Place Actors' listing Socket Actors")
<br>*Fig. 2.3.: Screenshot of Editor Tab 'Place Actors' listing Socket Actors*

<div style='page-break-after: always'></div>

#### 2.2.1. PUB-Socket Actor

A PUB-Socket Actor has:

* Variables, Group 'Endpoint':
  * Transport Type (`Select`): `tcp` (default), `inproc`
  * Host (`String`), default `127.0.0.1`
  * Port (`Integer`), default `5555`

![Screenshot of PUB-Socket Actor instance 'Details' panel with variables from Endpoint](Docs/ScreenshotPubSocketActor.jpg "Screenshot of PUB-Socket Actor instance 'Details' panel with variables from Endpoint")
<br>*Fig. 2.4.: Screenshot of PUB-Socket Actor instance 'Details' panel with variables from Endpoint*

<div style='page-break-after: always'></div>

* Functions:
  * `Open`, `IsOpen` (returns a `Boolean`)
  * `Bind` (in NNG known as *listen*), `IsBound` (returns a `Boolean`)
  * `Connect` (in NNG known as *dial*), `IsConnected` (returns a `Boolean`)
  * `IsLinked` (returns a `Boolean`)
  * `Close`
* Events (Delegates):
  * `OnOpen`, `OnOpenError` (returns an error message as `String`)
  * `OnBound`, `OnBindError` (returns an error message as `String`)
  * `OnConnected`, `OnConnectError` (returns an error message as `String`)
  * `OnLinked`, `OnLinkError` (returns an error message as `String`)
  * `OnClosed`, `OnCloseError` (returns an error message as `String`)

Upon successful `Open` or `Close`, the `OnOpen` or `OnClosed` event is triggered. Upon successful `Bind` or `Connect`, the `OnBound` or `OnConnected` event is triggered, as well as event `OnLinked` in both cases. If one the functions `IsBound` or `IsConnected` returns `true`, also the function `IsLinked` returns `true`.

![Screenshot of a Level Blueprint with function and event nodes of a PUB-Socket Actor](Docs/ScreenshotPubSocketActorFunctionAndEventNodes.jpg "Screenshot of a Level Blueprint with function and event nodes of a PUB-Socket Actor")
<br>*Fig. 2.5.: Screenshot of a Level Blueprint with function and event nodes of a PUB-Socket Actor*

<div style='page-break-after: always'></div>

#### 2.2.2. SUB-Socket Actor

A SUB-Socket Actor has:

* Variables, Group 'Endpoint':
  * Transport Type (`Select`): `tcp` (default), `inproc`
  * Host (`String`), default `127.0.0.1`
  * Port (`Integer`), default `5555`

![Screenshot of SUB-Socket Actor instance 'Details' panel with variables from Endpoint](Docs/ScreenshotSubSocketActor.jpg "Screenshot of SUB-Socket Actor instance 'Details' panel with variables from Endpoint")
<br>*Fig. 2.6.: Screenshot of SUB-Socket Actor instance 'Details' panel with variables from Endpoint*

<div style='page-break-after: always'></div>

* Functions:
  * `Open`, `IsOpen` (returns a `Boolean`)
  * `Bind` (in NNG known as *listen*), `IsBound` (returns a `Boolean`)
  * `Connect` (in NNG known as *dial*), `IsConnected` (returns a `Boolean`)
  * `IsLinked` (returns a `Boolean`)
  * `Close`
* Events (Delegates):
  * `OnOpen`, `OnOpenError` (returns an error message as `String`)
  * `OnBound`, `OnBindError` (returns an error message as `String`)
  * `OnConnected`, `OnConnectError` (returns an error message as `String`)
  * `OnLinked`, `OnLinkError` (returns an error message as `String`)
  * `OnClosed`, `OnCloseError` (returns an error message as `String`)

Upon successful `Open` or `Close`, the `OnOpen` or `OnClosed` event is triggered. Upon successful `Bind` or `Connect`, the `OnBound` or `OnConnected` event is triggered, as well as event `OnLinked` in both cases. If one the functions `IsBound` or `IsConnected` returns `true`, also the function `IsLinked` returns `true`. In addition a SUB-Socket Actor has a Blueprint-callable function `Receive` to trigger a message pickup.

![Screenshot of a Level Blueprint with function and event nodes of a SUB-Socket Actor](Docs/ScreenshotSubSocketActorFunctionAndEventNodes.jpg "Screenshot of a Level Blueprint with function and event nodes of a SUB-Socket Actor")
<br>*Fig. 2.7.: Screenshot of a Level Blueprint with function and event nodes of a SUB-Socket Actor*

<div style='page-break-after: always'></div>

### 2.3. Actor-Components

A Publisher Actor-Component or a Subscriber Actor-Component may be added to a Blueprints 'Components' tab by pressing the 'Add Components' button. The components are listed with category 'Messaging'.

![Screenshot of Actor-Components](Docs/ScreenshotActorComponents.jpg "Screenshot of Actor-Components")
<br>*Fig. 2.8.: Screenshot of Actor-Components listed in tab 'Components', category 'Messaging'*

#### 2.3.1. Publisher Actor-Component

A Publisher Actor-Component has:

* Variables:
  * Topic: The topic `String` with which is published
  * PUB-Socket: A PUB-Socket Actor instance which is used for publishing messages
* Functions:
  * `Publish` with in-parameter 'Message' as `String`
* Events (Delegates):
  * `OnPublished`

![Screenshot of Actor-Component Publisher](Docs/ScreenshotPublisher.jpg "Screenshot of Actor-Component Publisher")
<br>*Fig. 2.9.: Screenshot of an Event Graph with Publisher Actor-Component function and event nodes*

<div style='page-break-after: always'></div>

#### 2.3.2. Subscriber Actor-Component

A Subscriber Actor-Component has:

* Variables:
  * Topic: The topic `String` to subscribe to
  * Starts With (Check Box): If checked test whether received topic starts with the given topic &ndash; exact match otherwise (case sensitive string comparison in both cases)
  * SUB-Socket: A SUB-Socket Actor instance which is used for receiving messages
* Functions:
  * `Subscribe`
  * `IsSubscribed` (returns a `Boolean`)
  * `Unsubscribe`
* Events (Delegates):
  * `OnSubscribed`
  * `OnUnsubscribed`
  * `OnReceived` (returns the received 'Message' as `String`)

![Screenshot of Actor-Component Subscriber](Docs/ScreenshotSubscriber.jpg "Screenshot of Actor-Component Subscriber")
<br>*Fig. 2.10.: Screenshot of an Event Graph with Subscriber Actor-Component function and event nodes*

<div style='page-break-after: always'></div>

## 3. Demo

In the content browser enable the listing of plugin folders by checking `Settings > Show Engine Content` and `Settings > Show Plugin Content`. Find and navigate to folder 'Integration Tool Content'. The folder 'Demo' provides with three Blueprints BP_CubeCyan, BP_CubeYellow and BP_CubeGreen as well as with the level named Map_PubSub_Demo.

![Screenshot of Plugin Content](Docs/ScreenshotPluginContent.jpg "Screenshot of Plugin Content")
<br>*Fig. 3.1.: Screenshot of Content Browser with Integration Tool Content*

Usually, the PUB-SUB pattern is used to connect several endpoints of distributed systems whose applications run on different machines or nodes repectively. For the demo, we have endpoints in the same application for simplicity's sake &ndash; a PUB-Socket and a SUB-Socket are here in the level named Map_PubSub_Demo. The demo implements a PubSub-scheme as follows:

* A PUB-Socket Actor instance binds address `tcp://127.0.0.1:5555` (NNG: listen)
* A SUB-Socket Actor instance connects address `tcp://127.0.0.1:5555` (NNG: dial)
* Two Publisher Actor-Components (each in a cyan and a yellow cube) publish via the PUB-Socket Actor instance
* Two Subscriber Actor-Components (both in a green cube) subscribe via the SUB-Socket Actor instance

Please assure to have the corresponding firewall configured to allow traffic over localhost port 5555.

![Demo PubSub-Scheme](Docs/Demo-PubSub.jpg "Demo PubSub-Scheme")
<br>*Fig. 3.2.: Demo PubSub-Scheme*

<div style='page-break-after: always'></div>

The cyan and the yellow cube each use a Publisher Actor-Component and loop publishing a message `Hello from Cyan` with topic `Cyan` or `Hello from Yellow` with topic `Yellow` respectively.

![Screenshot of Blueprint BP_CubeCyan Event Graph](Docs/ScreenshotDemoActor_BP_CubeCyan.jpg "Screenshot of Blueprint BP_CubeCyan Event Graph")
<br>*Fig. 3.3.: Screenshot of Blueprint BP_CubeCyan Event Graph*

![Screenshot of Blueprint BP_CubeYellow Event Graph](Docs/ScreenshotDemoActor_BP_CubeYellow.jpg "Screenshot of Blueprint BP_CubeYellow Event Graph")
<br>*Fig. 3.4.: Screenshot of Blueprint BP_CubeYellow Event Graph*

<div style='page-break-after: always'></div>

A third, green cube uses two Subscriber Actor-Components to subscribe to topics `C` and `Y`&ndash;both check-boxes `Starts With` are checked&ndash;and appends the received messages to its `TextRender` Scene-Component and prints the same to the Output Log.

![Screenshot of Blueprint BP_CubeGreen Event Graph](Docs/ScreenshotDemoActor_BP_CubeGreen.jpg "Screenshot of Blueprint BP_CubeGreen Event Graph")
<br>*Fig. 3.5.: Screenshot of Blueprint BP_CubeGreen Event Graph*

The Map_PubSub_Demo has an instance each of PUB-Socket Actor and SUB-Socket Actor. The sockets link an endpoint with TCP on host 127.0.0.1 and port 5555 (default values).

![Screenshot of Demo Map](Docs/ScreenshotDemoMap.jpg "Screenshot of Demo Map")
<br>*Fig. 3.6.: Screenshot of Map_PubSub_Demo*

<div style='page-break-after: always'></div>

In the Level Blueprint, with `Event BeginPlay` the PUB-Socket Actor's function `Open` is called. With event `OnOpen (PUB-Socket)` the PUB-Socket Actor's function `Bind` is called.
With event `OnLinked (PUB-Socket)` the SUB-Socket Actor's function `Open` is called. With event `OnOpen (SUB-Socket)` the SUB-Socket Actor's function `Connect` is called. With event `OnLinked (SUB-Socket)` a timer based event starts a looped call of the SUB-Socket Actor's function `Receive` every other centisecond (`Time: 0.01`).

With `Event EndPlay` the Receive-Timer is cleard and invalidated, and the SUB-Socket Actor's as well as the PUB-Socket Actor's function `Close` is called.

![Screenshot of Demo Map Level-Blueprint](Docs/ScreenshotDemoLevelBlueprint.jpg "Screenshot of Demo Map Level-Blueprint")
<br>*Fig. 3.7.: Screenshot of Map_PubSub_Demo Level-Blueprint*

<div style='page-break-after: always'></div>

The demo map also has instances each of BP_CubeCyan, BP_CubeYellow, and BP_CubeGreen. In these cube instances, the Publisher and Subscriber Actor-Components were each assigned the  PUB-Socket Actor instance or the  SUB-Socket Actor instance, respectively.

![Screenshot of Blueprint BP_CubeCyan instance 'Details' panel](Docs/ScreenshotDemoActor_BP_CubeCyan_DetailsPanel.jpg "Screenshot of Blueprint BP_CubeCyan instance 'Details' panel")
<br>*Fig. 3.8.: Screenshot of BP_CubeCyan instances 'Details' panel, Publisher Actor-Component with assigned reference to a  PUB-Socket Actor instance*

![Screenshot of Blueprint BP_CubeYellow instance 'Details' panel](Docs/ScreenshotDemoActor_BP_CubeYellow_DetailsPanel.jpg "Screenshot of Blueprint BP_CubeYellow instance 'Details' panel")
<br>*Fig. 3.9.: Screenshot of BP_CubeYellow instances 'Details' panel, Publisher Actor-Component with assigned reference to a  PUB-Socket Actor instance*

<div style='page-break-after: always'></div>

![Screenshot of Blueprint BP_CubeGreen instance 'Details' panel 1](Docs/ScreenshotDemoActor_BP_CubeGreen_DetailsPanel_1.jpg "Screenshot of Blueprint BP_CubeGreen instance 'Details' panel 1")
<br>*Fig. 3.10.: Screenshot of BP_CubeGreen instance 'Details' panel, Subscriber Actor-Component 'Subscriber_C' with assigned reference to a  SUB-Socket Actor instance*

![Screenshot of Blueprint BP_CubeGreen instance 'Details' panel 2](Docs/ScreenshotDemoActor_BP_CubeGreen_DetailsPanel_2.jpg "Screenshot of Blueprint BP_CubeGreen instance 'Details' panel 2")
<br>*Fig. 3.11.: Screenshot of BP_CubeGreen instance 'Details' panel, Subscriber Actor-Component 'Subscriber_Y' with assigned reference to a  SUB-Socket Actor instance*

<div style='page-break-after: always'></div>

When the Map_PubSub_Demo level is open, click the Play button in the level editor to start Play-in-Editor PIE.

![Animation Screenshot of Demo Map PIE](Docs/DemoMapPIE.gif "Animation Screenshot of Demo Map PIE")
<br>*Fig. 3.12.: Animation Screenshot of Demo Map PIE*

The plugin writes to the output log with the custom log category LogNextGenMsg.

*Listing 3.1.: Output Log of Map_PubSub_Demo starting PIE*
```log
[...]
PIE: New page: PIE session: Map_PubSub_Demo ([...])
[...]
LogNextGenMsg: PubSocketActor1_2: Open socket ...
LogNextGenMsg: NngSocketObject_8: Socket successfully opened.
LogNextGenMsg: PubSocketActor1_2: Open socket done.
LogNextGenMsg: PubSocketActor1_2: Bind to tcp://127.0.0.1:5555 ...
LogNextGenMsg: NngSocketObject_8: Successfully listening to tcp://127.0.0.1:5555
LogNextGenMsg: PubSocketActor1_2: Bind done.
LogNextGenMsg: SubSocketActor1_2: Open socket ...
LogNextGenMsg: NngSocketObject_9: Socket successfully opened.
LogNextGenMsg: SubSocketActor1_2: Open socket done.
LogNextGenMsg: SubSocketActor1_2: Connect tcp://127.0.0.1:5555 ...
LogNextGenMsg: NngSocketObject_9: Successfully dialed tcp://127.0.0.1:5555
LogNextGenMsg: SubSocketActor1_2: Connect done.
LogNextGenMsg: BP_CubeGreen_2.Subscriber_C Subscribe topic C ...
LogNextGenMsg: SubSocketActor1_2 Subscribe topic 'C' ...
LogNextGenMsg: NngSocketObject_9: Topic successfully subscribed.
LogNextGenMsg: BP_CubeGreen_2.Subscriber_Y Subscribe topic Y ...
LogNextGenMsg: SubSocketActor1_2 Subscribe topic 'Y' ...
LogNextGenMsg: NngSocketObject_9: Topic successfully subscribed.
PIE: Server logged in
PIE: Play in editor total start time 0.182 seconds.
LogBlueprintUserMessages: [BP_CubeGreen_2] Hello from Yellow #0
LogBlueprintUserMessages: [BP_CubeGreen_2] Hello from Cyan #0
LogBlueprintUserMessages: [BP_CubeGreen_2] Hello from Yellow #1
LogBlueprintUserMessages: [BP_CubeGreen_2] Hello from Cyan #1
LogBlueprintUserMessages: [BP_CubeGreen_2] Hello from Cyan #2
LogBlueprintUserMessages: [BP_CubeGreen_2] Hello from Yellow #2
[...]
```

*Listing 3.2.: Output Log of Map_PubSub_Demo stopping PIE*
```log
[...]
LogWorld: BeginTearingDown for /IntegrationTool/Demo/Maps/UEDPIE_0_Map_PubSub_Demo
LogNextGenMsg: SubSocketActor1_2: Close socket ...
LogNextGenMsg: NngSocketObject_9: Topic 'C' successfully unsubscribed.
LogNextGenMsg: NngSocketObject_9: Topic 'Y' successfully unsubscribed.
LogNextGenMsg: NngSocketObject_9: Socket successfully closed.
LogNextGenMsg: SubSocketActor1_2: Close socket done.
LogNextGenMsg: PubSocketActor1_2: Close socket ...
LogNextGenMsg: NngSocketObject_8: Socket successfully closed.
LogNextGenMsg: PubSocketActor1_2: Close socket done.
[...]
```

<div style='page-break-after: always'></div>

## 4. Unsupported

Transport Protocol:

* `ipc://` &ndash; Inter Process Communication, aka UNIX domain socket
* `ws://` and `wss://` &ndash; WebSockets over TCP
* `tls://` &ndash; Transport Layer Security
* `zt://` &ndash; Communication over a ZeroTier&trade; network
* `mqtt://` &ndash; MQ Telemetry Transport (NNG&trade; does not provide with the MQTT transport protocol yet, neither does the plugin)

Communication Pattern:

* PAIR &ndash; simple one-to-one communication
* BUS &ndash; simple many-to-many communication
* REQREP &ndash; allows to build clusters of stateless services to process user requests
* PIPELINE &ndash; aggregates messages from multiple sources and load balances them among many destinations
* SURVEY &ndash; allows to query state of multiple applications in a single go

## A. Attribution

* The word mark *Unreal&reg;* and its logo are Epic Games, Inc. trademarks or registered trademarks in the US and elsewhere (cp. Branding Guidelines and Trademark Usage, URL: [https://www.unrealengine.com/en-US/branding](https://www.unrealengine.com/en-US/branding))
* The word marks *nanomsg&trade;* and *NNG&trade;* and its logos are trademarks of Garrett D'Amore, used with permission (cp. Trademark Policy, URL: [https://nanomsg.org/trademarks.html](https://nanomsg.org/trademarks.html))
* The word marks *EMQ&trade;*, *EMQX&trade;* and *NanoMQ&trade;* and its logos are trademarks of EMQ Technologies Co., Ltd.

## B. References

* *Unreal&reg; Engine Plugin: Integration Tool* by Roland Bruggmann aka brugr9 on Unreal&reg; Marketplace: [https://www.unrealengine.com/marketplace/en-US/product/integration-tool](https://www.unrealengine.com/marketplace/en-US/product/integration-tool)

## C. Citation

To acknowledge *"Unreal&reg; Engine Plugin: Integration Tool"* software, please cite

> Bruggmann, Roland (2022). *Unreal&reg; Engine Plugin: Integration Tool*, Version [#.#.#], UE [4.## or 5.#]. Unreal&reg; Marketplace. URL: [https://www.unrealengine.com/marketplace/en-US/product/integration-tool](https://www.unrealengine.com/marketplace/en-US/product/integration-tool). Copyright 2022 Roland Bruggmann aka brugr9. All Rights Reserved.

To acknowledge *"Unreal&reg; Engine Plugin: Integration Tool &mdash; Documentation"* (be it , e.g., the Readme or the Changelog), please cite

> Bruggmann, Roland (2022). *Unreal&reg; Engine Plugin: Integration Tool &mdash; Documentation*, \[Readme, Changelog\]. GitHub; accessed [Year Month Day]. URL: [https://github.com/brugr9/UEPluginIntegrationTool](https://github.com/brugr9/UEPluginIntegrationTool). Licensed under [Creative Commons Attribution-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/)

---
<!-- Footer -->

[![Creative Commons Attribution-ShareAlike 4.0 International License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

*"Unreal&reg; Engine Plugin: Integration Tool &ndash; Documentation"* &copy; 2022 by [Roland Bruggmann](https://about.me/rbruggmann) is licensed under [Creative Commons Attribution-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/)
