
## Leaflet.annnotate

A [LeafletJS](http://github.com/Leaflet/Leaflet) (0.7.x) extension for map mashups allowing authors to publish elements on their web map as structured data, accessible for major search engines and ultimately as re-usable parts of a composition (your geographical web map). We currently integrat the public [Schema.org vocabulary](http://schema.org) to expose your map elements as _typed_ information (e.g. as an [GovernmentOrganization](http://schema.org/GovernmentOrganization)) along with all latitude/longitude values of your _geo-referencing_.

Furthermore we allow you to refine your annotations of each map element through wrapping core elements of the [Dublin Core Metadata Element Set](http://dublincore.org/documents/dcmi-terms/) and/or through relying on parts of the current [HTML Standard](https://html.spec.whatwg.org/multipage/semantics.html).

### API

The following API options are available on standard Leaflet *Marker*, *CircleMarker* and at least a *GeoJSON Layer*. All these three type of overlays can be annotated in valid standard markup trough passing one or all of the following _key:value_ pairs as additional `options' to the object during creation:

| Option   | Expected Value | Definition |
|----------|:-------------:|:-------------:|
| `itemtype` (Mandatory) | Text | A Schema.org type name. Must be an URL conform text value without spaces. | 
| `geoprop` (Optional) | Text | Name of Schema.org property to use for the geographical indicator value of type _Place_. Default is "geo", which is valid for Place and all its subtypes. Other valid values consequently would be "location" (_Organisation_), "contentLocation" (_CreativeWork_), "locationCreated" (_CreativeWork_), etc. |
| `title` | Text | [name](http://schema.org/name) ([Thing](http://schema.org/Thing)), "The name of the item." |
| `description` | Text | [description](http://schema.org/description) ([Thing](http://schema.org/Thing)), "A short description of the item." |
| `alternateName` | Text | [alternateName](http://schema.org/alternateName) ([Thing](http://schema.org/Thing)), "An alias for the item." |
| `image` | Text | [alternateName](http://schema.org/alternateName) ([Thing](http://schema.org/Thing)), "An image of the item." Currently this should be an URL. |
| `sameAs` | Text | [sameAs](http://schema.org/sameAs) ([Thing](http://schema.org/Thing)), "URL of a reference Web page that unambiguously indicates the item's identity. E.g. the URL of the item's Wikipedia page, Freebase page, or official website." |
| `url` | Text | [url](http://schema.org/url) ([Thing](http://schema.org/Thing)), "URL of the item." |
| `creator` | Text | [creator](http://purl.org/dc/elements/1.1/creator) (Dublin Core), "An entity primarily responsible for making the resource. Typically, the name of a Creator should be used to indicate the entity (e.g. person, organisation or service)." |
| `contributor` | Text | [contributor](http://purl.org/dc/elements/1.1/contributor) (Dublin Core), "An entity responsible for making contributions to the resource. Typically, the name of a Contributor should be used to indicate the entity (e.g. person, organisation or service)." |
| `publisher` | Text | [publisher](http://purl.org/dc/elements/1.1/publisher) (Dublin Core), "An entity responsible for making the resource available. Typically, the name of a Contributor should be used to indicate the entity (e.g. person, organisation or service)." |
| `published` | Text and Integers | [date](http://purl.org/dc/elements/1.1/date) (Dublin Core), "A point or period of time. May be used to express temporal information at any level of granularity. Recommended best practice is to use an encoding scheme, such as the [W3CDTF](http://www.w3.org/TR/NOTE-datetime) profile of ISO 8601." |
| `identifier` | Text | [identifier](http://purl.org/dc/elements/1.1/identifier) (Dublin Core), "An unambiguous reference to the resource within a given context. Recommended best practice is to identify the resource by means of a string conforming to a formal identification system." |
| `rights` | Text | [rights](http://purl.org/dc/elements/1.1/rights) (Dublin Core), "Information about rights held in and over the resource. Typically, rights information includes a statement about various property rights associated with the resource, including intellectual property rights." |
| `derivedFrom` | Text | [source](http://purl.org/dc/elements/1.1/source) (Dublin Core), "A related resource from which the described resource is derived. The described resource may be derived from the related resource in whole or in part. Recommended best practice is to identify the related resource by means of a string conforming to a formal identification system." |
| `format` | Text | [format](http://purl.org/dc/elements/1.1/format) (Dublin Core), "The file format, physical medium, or dimensions of the resource. Recommended best practice is to use a controlled vocabulary such as the list of Internet Media Types [MIME](http://www.iana.org/assignments/media-types/)." |
| `language` | Text | [language](http://purl.org/dc/elements/1.1/language) (Dublin Core), "A language of the resource. Recommended best practice is to use a controlled vocabulary such as RFC 4646 [RFC4646](http://www.ietf.org/rfc/rfc4646.txt)." |
| `created` | Text and Integers | [created](http://purl.org/dc/terms/created) (Dublin Core Term), "Date of creation of the resource. Recommended best practice is to use an encoding scheme, such as the [W3CDTF](http://www.w3.org/TR/NOTE-datetime) profile of ISO 8601." |
| `modified` | Text and Integers | [modified](http://purl.org/dc/terms/modified) (Dublin Core Term), "Date on which the resource was changed.. Recommended best practice is to use an encoding scheme, such as the [W3CDTF](http://www.w3.org/TR/NOTE-datetime) profile of ISO 8601." |

Note: Contrary to the standard specification, one option (`key`) can be annotated just once, for example currently this API does not enable you two specify two `alternateNames` for your map element.

### Examples - Building anotations using the API

Include the following script from this repository in your HTML file:
<pre>
	<script src="Leaflet.annotate.Microdata.js"></script>
</pre>

After that, if you pass `itemtype` as an option to your map element during creation, it is configured for annotation. Annnotation will happen if you add the map element to your Leaflet `map` object.

Example1: Annotating a *Marker* as map element representing a `City` looks like this:
<pre>
var marker = L.marker([40.573112, -73.980740], { itemtype: 'City', title: 'New York City'})
</pre>
This also exposes your markers location values as machine readable [GeoCoordinates](http://schema.org/GeoCoordinates) values in HTML.

Example2: Annotating a *Circle Marker* (SVG) to represent a [Creative Work](http://schema.org/CreativeWork), a virtual Poem.
<pre>
var circleMarker = L.circleMarker([40.573112, -73.980740], { itemtype: 'CreativeWork', geoprop: 'locationCreated'
    title: 'The circle marker stating where this meta poem was created.'
})
This too exposes this markers location value as machine readable [GeoCoordinates](http://schema.org/GeoCoordinates) but more specifically, it exposes the location as the _Place_ where the poem was written (`locationCreated`).
</pre>

Example3: Annotating a group of geometries (SVG) which all represent the boundaries of one [Country](http://schema.org/Country), in this case the (formal) boundaries of the "Estados Unidos":
<pre>
var statesBoundaries = L.geoJson(response, { itemtype: 'Country',
    title: 'Estados Unidos'
}).addTo(map)
// Note: Here we must call annotate() explicitly on geographical Overlays such as this GeoJSON Layer
statesBoundaries.annotate()
</pre>

Note: Here an additional and explicit call of _annotate()_ is necessary.<br/>
This exposes the polygons location values (all GeoCoordinates of the boundaries) as a machine readable [GeoShape](http://schema.org/GeoShape).

### Background

The envisioned application for revised markup in geographic web maps is (besides all third-party use cases enabled through providing structured data):<br/>A digital map legend interactively assisting users in analyzing the contents and the scope of so called ``map mashups''.

### Implementation Notes

To annotate SVG Elements we introduce a `metadata` Element next to the respective `path`. In practice both are often grouped within a `g` element. All Schema.org and Dublin Core based annotation values are attributes of a `meta` element.

Maps rendered in the **Internet Explorer** along with _VML_ is currently **not supported** when deploying Leaflet.annnotate.

At the moment all geographic annotations (of types which are not a sub-type of _Place_) are done through introducing an extra _Place_ element (and are not possible using just a _GeoShape_).

### Release History

0.3, *Upcoming*

 * Annotatable Leaflet Items: UI Layers (Marker, Popup), Vector Layers (CircleMarker), Other Layers (GeoJSON)
 * Compatible with the latest stable LeafletJS Release: 0.7.7
 * Most probably also compatible from at least all versions of 0.7.3 and upwards

Author:<br/>
Malte Reißig (2016

