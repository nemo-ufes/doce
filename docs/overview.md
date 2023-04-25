## Usage Guide <a name="usageguide"></a>

### Table of Contents

* 1\. [Overview](#overview)
* 2\. [Research Activities](#activities)
* 3\. [Agents and Devices](#agentsdevices)
* 4\. [Location and Geographical Features](#location)
* 5\. [Material Entities of Interest](#material)
* 6\. [Water Qualities and their Types](#qualities)
* 7\. [Measurement (incl. units)](#measurement)
* 8\. [Environmental Monitoring](#monitoring)
    * 8.1\. [Monitoring Program](#program)
    * 8.2\. [Environmental Quality Norms](#norms)

See also [reference table of contents](#toc).

<a name="overview"></a>

### 1. Overview

This section provides an introduction to *doce* as a primer to prospective users. [Turtle](https://www.w3.org/TR/turtle/) is used as a syntax for RDF serialization for illustrative purposes.  

An overview of the main fragments of the doce taxonomy is shown below, anchored in the gUFO implementation of UFO. `gufo` is a prefix for the namespace: http://purl.org/nemo/gufo#

The taxonomy of events of doce concerns research activities of interest:

> 	* gufo:Event
> 		* ResearchActivity
> 			* Sampling
> 			* Preparation
> 			* Measurement

The taxonomy of objects concern the agents, devices, material entities and procedures that participate in research activities:

> 	* gufo:Object
> 		* Agent
> 			* ResearchActivityAgent
> 			* ResearchActivityPrincipal
> 		* Device
> 		* MaterialEntity
> 			* AbioticEntity
> 				* AmountOfWater
> 					* WaterSample
> 						* SurfaceWaterSample
> 				* GeographicFeature
> 					* ArtificialGeographicFeature
> 						* Reservoir
> 					* HydrographicFeature
> 						* SurfaceWaterFeature
> 							* Lake
> 							* Reservoir
> 							* River
> 					* NaturalGeographicFeature
> 						* Lake
> 						* River
> 			* BioticEntity
> 			* Sample
> 				* WaterSample
> 					* SurfaceWaterSample
> 			* SampledEntity
> 		* ResearchActivityProcedure

Abstract individuals are used to establish units of measurement and geolocation coordinates:

> 	* gufo:AbstractIndividual
> 		* Unit
> 		* gufo:QualityValue
> 			* GeographicPoint

The taxonomy of qualities concerns those aspects of the material entities that are subject to measurement and further investigation. A number of qualities are defined in doce to facilitate integration:

> 	* gufo:Quality
> 		* MetereologicalQuality
> 			* AmbientTemperature
> 			* Rainfall
> 			* RelativeHumidity
> 		* PhysicalChemicalQuality
> 			* BiologicalOxygenDemand
> 			* Concentration
> 				* MetalConcentration
> 				* MetalloidConcentration
> 				* NonMetalConcentration
> 				* SolidsConcentration
> 			* Conductivity
> 			* Hardness_CalciumCarbonateEquivalence
> 			* pH
> 			* RedoxPotential
> 			* Salinity
> 			* SecchiDiskTransparency
> 			* Temperature
> 			* TotalAlkalinityAsCaCO3
> 			* TrueColor
> 			* Turbidity
> 		* SamplingDepth
> 		* VolumePerUnitTime
> 			* FlowRate

Additional (metereological, biological and physical-chemical) qualities may be defined instantiating the taxonomy of Quality Kinds:

> 	* gufo:Type
> 		* gufo:RigidType
> 			* gufo:Kind
> 				* QualityKind
> 					* MetereologicalQualityKind
> 					* WaterQualityKind
> 						* BiologicalQualityKind
> 						* PhysicalChemicalQualityKind
> 							* ChemicalEntityConcentrationQualityKind
> 			* ChemicalEntityType

Doce is anchored in gUFO in the following ways:

1. Doce (as a UFO-based ontology) *specializes* gUFO classes in the gUFO taxonomy of individuals  
   For example, `doce:Device rdfs:subClassOf gufo:Object`, `doce:Measurement rdfs:subClassOf gufo:Event` and `doce:WaterQuality rdfs:subClassOf gufo:Quality`.
2. Doce *specializes* gUFO classes in the gUFO taxonomy of types  
   For example, `doce:QualityKind rdfs:subClassOf gufo:Kind`.
3. Doce *instantiates* gUFO classes in the taxonomy of types  
   For example, `doce:Agent rdf:type gufo:Category`, `doce:SampledEntity rdf:type gufo:Role`.

Doce reuses some units of measurement QUDT 2.1 vocabularies. Correspondences are indicated when necessary, and the `unit` prefix is used for namespace http://qudt.org/vocab/unit/.  Some correspondences to QUDT Quantity Kinds are also identified. Correspondences to  Chemical Entities of Biological Interest (CHEBI) <https://www.ebi.ac.uk/ols/ontologies/chebi> are also indicated. 


<a name="activities"></a>

### 2. Research Activities

The taxonomy of research activities has the following overall structure:

> 	* gufo:Event
> 		* ResearchActivity
> 			* Sampling
> 			* Preparation
> 			* Measurement

A doce:ResearchActivity is a gufo:Event which is conducted by one or more agents in order to investigate one or more aspects of a phenomenon of interest.

Since all research activities are events they can be given temporal boundaries. gUFO includes a number of data and object properties that can be used with events. For example, temporal aspects can be captured with the data properties gufo:hasBeginPointInXSDDate, gufo:hasBeginPointInXSDDateTimeStamp, gufo:hasEndPointInXSDDate and gufo:hasEndPointInXSDDateTimeStamp. These properties determine the time point when the event starts to take place or when it ends. (Instantaneous events can be represented with the same begin and end point.) (Temporal instants may also be reified using time:Instant in which case the gufo:hasBeginPoint and gufo:hasEndPoint object properties are applicable, see [OWL-Time](https://www.w3.org/TR/owl-time/) for details concerning time:Instant, including support for Allen relations, temporal reference systems, temporal precision.)

Further, a doce:ResearchActivity is a doce:GeographicallyLocatedEntity, and hence can be located in space (see the doce:locatedIn object property).

The following example shows a fragment representing the sampling of water in a geographic point named RCA-01, on 17th August 2009, 14:24 Brasília Time.

    :WaterSampling314020-2017-1 rdf:type owl:NamedIndividual ,
                                        doce:Sampling ;
                                doce:locatedIn :RCA-01 ;
                                gufo:hasBeginPointInXSDDateTimeStamp "2009-08-17T14:24:00-03:00"^^xsd:dateTimeStamp ;
                                gufo:hasEndPointInXSDDateTimeStamp "2009-08-17T14:24:00-03:00"^^xsd:dateTimeStamp .

The following fragment shows the doce:Sample created in the sampling above. A doce:Sample represents a doce:SampledEntity. In this particular case, the sample entity is the Rio Doce (which is previously defined in doce with the URI doce:RioDoce).

    :WaterSample314020-2017-1 rdf:type owl:NamedIndividual ,
                                    doce:SurfaceWaterSample ;
                            doce:represents doce:RioDoce ;
                            gufo:wasCreatedIn :WaterSampling314020-2017-1 .

The created Sample is typically subject to measurement later, which can be representing using doce:Measurement. A doce:Measurement is a doce:ResearchActivity whose purpose is to ascertain the value of a quality of an entity of interest (a gufo:Object).

The following example represents the (ex-situ) measurement of alkalinity (doce:TotalAlkalinityAsCaCO3) of the sample identified earlier (which is identified using the doce:measured object property). The measurement is expressed in milligrams per liter (unit:MilliGM-PER-L). (See [more on units](#measurement).) The measured value is represented with the gufo:hasQualityValue data property. In this case, the water sample is found to have a total alkalinity of 22.0 milligrams per liter:

    :WaterMeasurementAlkalinity001 rdf:type owl:NamedIndividual ,
                                            doce:Measurement ;
                                doce:measured :WaterSample314020-2017-1 ;
                                doce:expressedIn unit:MilliGM-PER-L ;
                                doce:measuredQualityKind doce:TotalAlkalinityAsCaCO3 ;
                                gufo:hasQualityValue "22.0"^^xsd:double .

For an in-situ doce:Measurement, the measured gufo:Object is often a doce:GeographicFeature, such as a river of a lake. Note that for an in-situ measurement of a doce:MetereologicalQuality, no specific gufo:Object is indicated.

The following fragment represents the (in-situ) measurement of water temperature (no sampling involved in this case.) The location of the measurement itself is now relevant (in this case point RCA-01). The quality kind measured is doce:Temperature and the measurement is expressed in degrees Celsius (unit:DEC_C). (See [more on units](#measurement).) The measured value is represented with the gufo:hasQualityValue data property. In this case, the water in point RCA-01 measured 21.21 degrees Celsius on the 17th August 2009 at 14:24 Brasília Time:

    :WaterTemperatureMeasurement314020-2017-1 rdf:type owl:NamedIndividual ,
                                                    doce:Measurement ;
                                            doce:locatedIn :RCA-01 ;
                                            doce:measured doce:RioDoce ;
                                            doce:measuredQualityKind doce:Temperature ;
                                            doce:expressedIn unit:DEG_C ;
                                            gufo:hasBeginPointInXSDDateTimeStamp "2009-08-17T14:24:00-03:00"^^xsd:dateTimeStamp ;
                                            gufo:hasEndPointInXSDDateTimeStamp "2009-08-17T14:24:00-03:00"^^xsd:dateTimeStamp ;
                                            gufo:hasQualityValue "21.21"^^xsd:double .

The following fragment represents the (in-situ) measurement of water transparency in the same location and time (again no sampling involved in this case.) Transparency is measured in meters (unit:M).

    :WaterTransparencyMeasurement314020-2017-1 rdf:type owl:NamedIndividual ,
                                                        doce:Measurement ;
                                            doce:locatedIn :RCA-01 ;
                                            doce:measured doce:RioDoce ;
                                            doce:measuredQualityKind doce:SecchiDiskTransparency ;
                                            doce:expressedIn unit:M ;
                                            gufo:hasBeginPointInXSDDateTimeStamp "2009-08-17T14:24:00-03:00"^^xsd:dateTimeStamp ;
                                            gufo:hasEndPointInXSDDateTimeStamp "2009-08-17T14:24:00-03:00"^^xsd:dateTimeStamp ;
                                            gufo:hasQualityValue "0.43"^^xsd:double .
 
Finally, a doce:Preparation activity is a doce:ResearchActivity whose purpose is to prepare a sample for further analysis. For example, consider activities such as "extractions" in the preparation for measuring the concentration of metals in a water sample. In many cases, preparation is not explicitly modeled, and the preparation activities are encompassed in measurement. When preparation is explicit, a doce:Preparation terminates an original doce:Sample, creating a newly prepared doce:Sample. (See gufo:wasTerminatedIn and gufo:wasCreatedIn.) Explicit preparation creates the opportunity to track the agents involved in handling preparation, the procedures and devices employed.


<a name="agentsdevices"></a>

### 3. Agents and Devices

TODO

Example of a doce:ResearchActivityPrincipal responsible for delegating the sampling and measurement activities discussed earlier:

    :Renova rdf:type owl:NamedIndividual ,
                    doce:Agent ;
            doce:delegated :WaterMeasurementAlkalinity001 ,
                        :WaterSampling314020-2017-1 ,
                        :WaterTemperatureMeasurement314020-2017-1 ,
                        :WaterTransparencyMeasurement314020-2017-1 .


<a name="location"></a>

### 4. Location and Geographical Features

TODO

Example of a doce:GeographicPoint where the sampling and in-situ measurements took place:

    :RCA-01 rdf:type owl:NamedIndividual ,
                    doce:GeographicPoint ;
            wgs:lat "-20.3471"^^xsd:float ;
            wgs:long "-43.1127"^^xsd:float ;
            rdfs:comment "Ponte férrea sobre o rio do Carmo, em Acaiaca (MG). Não atingido pelo rejeito. Área de pastagem." ;
            rdfs:label "Acaiaca - Carmo 01" .



<a name="material"></a>

### 5. Material Entities of Interest

TODO

<a name="qualities"></a>

### 6. Water Qualities and their Types

TODO

<a name="measurement"></a>

### 7. Measurement (incl. units)

TODO

<a name="monitoring"></a>

### 8. Environmental Monitoring

TODO

<a name="program"></a>

### 8.1. Monitoring Program

TODO

<a name="norms"></a>

### 8.2. Environmental Quality Norms

TODO

 

[gufo:Individual]: #Individual
[individual]: #Individual
[individuals]: #Individual
[gufo:Situation]: #Situation
[gufo:isProperPartOf]: #isProperPartOf
[gufo:RelationshipType]: #RelationshipType
[Casati and Varzi, 2015]: https://plato.stanford.edu/archives/win2015/entries/events/
[Guizzardi et al., 2018]: https://doi.org/10.1007/978-3-030-00847-5_12
[Guizzardi, 2005]: https://research.utwente.nl/en/publications/ontological-foundations-for-structural-conceptual-models
[Carvalho et al., 2017]: <http://dx.doi.org/10.1016/j.datak.2017.03.002>

