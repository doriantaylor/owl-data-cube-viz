## Data Cube Visualization Ontology

<div class="section" about="#" typeof="owl:Ontology bibo:Webpage">

  - Author  
    [<span property="foaf:name">Dorian
    Taylor</span>](http://doriantaylor.com/person/dorian-taylor#me)
  - Version  
    0.1
  - Created  
    October 12, 2016
  - Namespace URI  
    <https://vocab.methodandstructure.com/data-cube-viz#>
  - Preferred Namespace Prefix  
    qbv

This is a small vocabulary that augments RDF Data Cube with a mechanism
for matching data visualizations.

Context: we *have* a [Data
Cube](https://www.w3.org/TR/vocab-data-cube/ "The RDF Data Cube Vocabulary")
resource and we *want* to apply the appropriate visualization to it.
Rather, we may have *several* of these resources and we want to dispatch
them to one or more visualization mechanisms.

This document does not specify the visualization mechanism, it merely
points to it. Indeed, the mechanism need not strictly be
*visualization*, but rather any kind of representational transformation.

We can imagine, much like we do with
[CSS](https://www.w3.org/Style/CSS/ "Cascading Style Sheets") or
[Fresnel](https://www.w3.org/2005/04/fresnel-info/), using the identity,
structure and content of the source data to inform the dispatching. Let
us begin by taking a census of the different features of Data Cube:

  - The classes, `qb:DataSet`, `qb:ObservationGroup` and `qb:Slice`,
  - Subclasses of these classes,
  - Individual instances of these classes.

This is a good start, however we are much more likely to encounter
instances of `qb:DataSet` proper, which only vary in terms of their
`qb:DataStructureDefinition`. Therefore:

  - References to specific `qb:DataStructureDefinition` resources,
  - `qb:Slice` resources that reference a particular `qb:SliceKey`.

Still, up to this point, the selection criteria all depend on knowing
the identity of the resource, class, data structure, or slice key. In
the likely case that any of these are generated programmatically, it
will be necessary to match visualizations based on *content*. For
example:

  - If a structure has one `qb:dimension`, dispatch to visualization
    `A`; if two, dispatch to `B`; otherwise leave it alone
  - Or if a `qb:dimension` is known to be an interval or datetime, then
    dispatch to a [time
    series](https://en.wikipedia.org/wiki/Time_series "Time series — Wikipedia")
  - Or if a `qb:dimension` is a zone on a map, forward to a
    [choropleth](https://en.wikipedia.org/wiki/Choropleth_map "Choropleth map — Wikipedia")
  - Or if `qb:measure` properties contain descriptive statistics, then
    dispatch to a [box
    plot](https://en.wikipedia.org/wiki/Box_plot "Box plot — Wikipedia")
  - Or if the resource being rendered is a `qb:Slice` that fixes a
    particular dimension, like the month of a year, forward to a
    year-over-year time series…

As such, we need to be able to dispatch based on the quantity and
content of `qb:dimension`, `qb:measure` and `qb:attribute` properties.

<div class="section">

#### TODO

  - Describe a candidate selection algorithm
  - Handle qb:attribute/qb:AttributeProperty meaningfully
  - Some kind of proscription or negation mechanism, to specify rules we
    *don't* want to match

</div>

As usual, the master machine-readable spec is embedded in this document,
and there are [RDF/XML](https://vocab.methodandstructure.com/1.rdf) and
[N3/Turtle](https://vocab.methodandstructure.com/1.n3) variants.

</div>

<div class="section">

## Classes

![](https://vocab.methodandstructure.com/data-cube-viz-classes.svg)

<div id="Selector" class="section" about="[qbv:Selector]" typeof="owl:Class">

#### Selector

The selector is the main entity that ties together the instructions for
matching data sets with the processing target.

> Since a `qbv:Selector` is a `qb:ComponentSet`, we can reuse
> `qb:dimension`, `qb:measure` and `qb:attribute`.

  - Subclass of:  
    [qb:ComponentSet](https://www.w3.org/TR/vocab-data-cube/#dfn-qb-componentset)
  - In domain of:  
    [qbv:class](https://vocab.methodandstructure.com/data-cube-viz#class)
    [qbv:instance](https://vocab.methodandstructure.com/data-cube-viz#instance)
    [qbv:structure](https://vocab.methodandstructure.com/data-cube-viz#structure)
    [qbv:target](https://vocab.methodandstructure.com/data-cube-viz#target)
    [qbv:fix](https://vocab.methodandstructure.com/data-cube-viz#fix)
    [qbv:dimensions](https://vocab.methodandstructure.com/data-cube-viz#dimensions)
    [qbv:min-dimensions](https://vocab.methodandstructure.com/data-cube-viz#min-dimensions)
    [qbv:max-dimensions](https://vocab.methodandstructure.com/data-cube-viz#max-dimensions)

</div>

</div>

<div class="section">

## Properties

### Object Properties

<div id="class" class="section" about="[qbv:class]" typeof="owl:ObjectProperty">

#### class

Designates a class that should be chosen by this selector.

> If unspecified, we should assume the selector matches the union of
> `qb:DataSet`, `qb:Slice`, and `qb:ObservationGroup`.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)
  - Range:  
    [rdfs:Class](https://www.w3.org/TR/rdf-schema/#ch_class)

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

<div id="instance" class="section" about="[qbv:instance]" typeof="owl:ObjectProperty">

#### instance

Designates an explicit resource which will be the data source for the
selector.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

<div id="structure" class="section" about="[qbv:structure]" typeof="owl:ObjectProperty">

#### structure

Designates an explicit resource which will be the data source for the
selector.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)
  - Range:  
    <span about="_:uc" typeof="owl:Class">
    <span data-inlist="" property="owl:unionOf" resource="http://purl.org/linked-data/cube#DataStructureDefinition">
    [qb:DataStructureDefinition](https://www.w3.org/TR/vocab-data-cube/#dsd)
    </span> ∪
    <span data-inlist="" property="owl:unionOf" resource="http://purl.org/linked-data/cube#SliceKey">
    [qb:SliceKey](https://www.w3.org/TR/vocab-data-cube/#slices) </span>
    </span>

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

<div id="target" class="section" about="[qbv:target]" typeof="owl:FunctionalProperty owl:ObjectProperty">

#### target

Designates the target of the selector, the resource that will perform
the operation. This resource can be anything (e.g. a program, or an XSLT
transform).

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

<div id="fix" class="section" about="[qbv:fix]" typeof="owl:ObjectProperty">

#### fix

Designates a component property which is fixed in a qb:Slice.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)
  - Range:  
    [qb:ComponentProperty](https://www.w3.org/TR/vocab-data-cube/#dsd-dimensions)

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

### Datatype Properties

<div id="dimensions" class="section" about="[qbv:dimensions]" typeof="owl:DatatypeProperty">

#### dimensions

Specifies the exact number of data dimensions the target is capable of
rendering.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)
  - Range:  
    [xsd:positiveInteger](http://www.w3.org/TR/xmlschema-2/#positiveInteger)
  - Cardinality:  
    1

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

<div id="min-dimensions" class="section" about="[qbv:min-dimensions]" typeof="owl:DatatypeProperty">

#### min-dimensions

Specifies the minimum number of data dimensions the target is capable of
rendering.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)
  - Range:  
    [xsd:positiveInteger](http://www.w3.org/TR/xmlschema-2/#positiveInteger)
  - Cardinality:  
    1

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

<div id="max-dimensions" class="section" about="[qbv:max-dimensions]" typeof="owl:DatatypeProperty">

#### max-dimensions

Specifies the maximum number of data dimensions the target is capable of
rendering.

  - Domain:  
    [qbv:Selector](https://vocab.methodandstructure.com/data-cube-viz#Selector)
  - Range:  
    [xsd:positiveInteger](http://www.w3.org/TR/xmlschema-2/#positiveInteger)
  - Cardinality:  
    1

[Back to Top](https://vocab.methodandstructure.com/data-cube-viz#)

</div>

</div>

<div class="section">

## References

  - [The RDF Data Cube
    Vocabulary](https://www.w3.org/TR/vocab-data-cube/)
  - [Cascading Style Sheets](https://www.w3.org/Style/CSS/)
  - [Fresnel - Display Vocabulary for
    RDF](https://www.w3.org/2005/04/fresnel-info/)

</div>
