# Interface layers

Interface layers are perhaps the most misunderstood type of layer, and are
responsible for the communication that transpires over a relation between two
services. This type of layer encapsulates a single “interface protocol” and is
generally written and maintained by the author of the primary charm that
provides that interface. However, it does cover both sides (provides and
requires) of the relation and turns the two-way key-value store that are Juju
relations under-the-hood into a full-fledged API for interacting with charms
supporting that interface.

It is important to note that interface layers **do not** actually implement
either side of the relation. Instead, they are solely responsible for the
**communication** that goes on over the relation, relying on charms on either
end to decide what to do with the results of that communication.

Interface layers currently must be written in Python and extend the ReactiveBase
class, though they can then be **used** by any language using the built-in CLI API.

## Design considerations

 When writing an interface, there is a small amount of pre-planning into what
that interface should look like in terms of the communication between
services/unit(s) participating in the relationship.

Common questions to answer:

- What units need to participate in the conversation?

- What data is being sent on the wire?

- Are we sending data that is essentially static to any service connecting over
this interface?

- How should this data be made available to the requirer?

- What states should this interface raise on the provider?

- What states (if any) should this interface raise on the requirer?


## The Relation Lifecycle

Before writing any interface code, its also important to understand the
relationship lifecycle and how this impacts the communication events happening
between units participating in the conversation.

### Relation Scopes

An interfaces relationship scope falls into one of two categories. An implied
`global` scope, which is the default for all interfaces. Or a `container` scope
which is explicitly defined. That is to say: a globally-scoped relation has a
single unit scope, whilst a container-scoped relation has one for each principal
unit.

You will typically only require a `container` scoped relation when programming
an interface for a  subordinate charm, which by the nature of subordinates is
co-located with the principal service it is related to on this interface.

> Relation Scopes have nothing to do with communication scopes found when
programming interface layers. More on that topic [here](develoer-layers-interfaces2.html#communication-scopes)

### Relationship Lifecycle

Relationships also follow the traditional [event
cycle](developer-event-cycle.html#relation-events-by-example) as outlined in the
event cycle documentation. Ensure you're familiar with the event hooks that will
be triggered during a relationship joining the conversation, participating, and
what happens when the relationship is broken.


### Handling Relation data

When a relationship is first established, its not uncommon that one side has
not prepared the proper relation data on the first hook execution of
[name]-relation-changed. The only implicitly defined data point that will be
sent is `private-address`, if the relation depends on additional data, the
interface author will need to ensure they have guarded against taking action
until the proper relation data has been made available. Which will be triggered
by any participating unit assigning data on the relation interface that was not
previously defined, which will in turn trigger [name]-relation-changed to
execute again.


## Communication Scopes

Read on about [Communication Scopes](developer-layers-interfaces2.html)
