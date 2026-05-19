---
title: SST::Event::Handler
---

```cpp
void handler(SST::Event* event);
void handler(SST::Event* event, dataT data);
```

Event handlers are used by Links when an event arrives on the Link. The handler passes in the event as well as optional data if the handler was registered with an extra field. The handler does not return anything.

## Parameters
* **event** (SST::Event) The event to be handled
* **data** (dataT) Optional, data that the handler was created with.
* **returns** none

## Constructing Handlers
An event handler belonging to class `class` and pointing to function `func` is constructed as follows. The second line demonstrates a handler with metadata of type `dataT` and a value of `data`.
```cpp
SST::Handler* handler = new Event::Handler<class, &class::func>(this);
SST::Handler* handler = new Event::Handler<class, &class::func, dataT>(this, data);
```

:::info
This definition was changed as of SST 14.0 due to the reintroduction of checkpointing support. In SST 16.0, the old style handler is no longer available and the temporary name `Handler` that was introduced in SST 14.0 is an alias for the above new style Handler. The name `Handler2` is now deprecated, completing the transition.

```cpp title="Handler construction in different versions of SST"
/* Pre-SST 14.0 handler - not checkpointable */
SST::Handler* handler = new Event::Handler<class>(this, &class::func);
SST::Handler* handler = new Event::Handler<class, metaT>(this, &class::func, data);

/* SST 14.x and 15.x - old and new style supported */
// Old style, deprecated and not checkpointable - update to Handler2 style instead
SST::Handler* handler = new Event::Handler<class>(this, &class::func);
SST::Handler* handler = new Event::Handler<class, dataT>(this, &class::func, data);
// New style, checkpointable
SST::Handler* handler = new Event::Handler2<class, &class::func>(this);
SST::Handler* handler = new Event::Handler2<class, &class::func, dataT>(this, data);

/* SST 16.0 and beyond - only new checkpointable style supported */
SST::Handler* handler = new Event::Handler<class, &class::func>(this);
SST::Handler* handler = new Event::Handler<class, &class::func, dataT>(this, data);
// The Handler2 name is deprecated, use Handler instead
SST::Handler* handler = new Event::Handler2<class, &class::func>(this);
SST::Handler* handler = new Event::Handler2<class, &class::func, dataT>(this, data);
```

:::

## Example
<!--- SOURCE_CODE: sst-elements/src/sst/elements/simpleElementExample/basicLinks.h --->
<!--- SOURCE_CODE: sst-elements/src/sst/elements/simpleElementExample/basicLinks.cc --->
```cpp title="Excerpt from sst-elements/src/sst/elements/simpleElementExample/basicLinks.h"
class basicLinks : public SST::Component
{
public:
    /* ELI and unrelated functions omitted for brevity */
    basicLinks(SST::ComponentId_t id, SST::Params& params);
private:
    // An event handler
    void handleEvent(SST::Event* ev);

    // An event handler with an ID that indicates
    // which link sent the event
    void handleEventWithID(SST::Event* ev, int linknum);
} 
```
```cpp title="Excerpt from sst-elements/src/sst/elements/simpleElementExample/basicLinks.cc"
basicLinks::basicLinks(ComponentId_t id, Params& params) : Component(id)
{
    // Configure the link connected to our 'port_handler' port to call handleEvent when an event arrives
    configureLink("port_handler", new Event::Handler<basicLinks>, this, &basicLinks::handleEvent));

    std::string linkname = "port_vector0";
    int portnum = 0;
    while (isPortConnected(linkname)) {
        // Configure links connected to port_vector* to call handleEventWithID when an event arrives
        // Also pass the index of the port/link that received the event
        configureLink(linkname, new Event::Handler<basicLinks, &basicLinks::handleEventWithID, int>(this, portnum));

        portnum++;
        linkname = "port_vector" + std::to_string(portnum);
    }

    /* Rest of configuration */
}

// Just count how many events arrive
void basicLinks::handleEvent(SST::Event* event)
{
    events_received++;
    delete event;
}

// Count how many events arrive on each port
void basicLinks::handleEventWithID(SST::Event *event, int id)
{
    events_received_vector[i]++;
    delete event;
}
```

## Header
```cpp
#include <sst/core/component.h> // or
#include <sst/core/subcomponent.h> // or
#include <sst/core/componentextension.h> // or
#include <sst/core/event.h>
```
