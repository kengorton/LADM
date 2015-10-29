Overview:
The LADM schema for the Colombia Pilot project was implemented in an ArcGIS Workspace with the purpose of representing the relevant and essential characteristics of the original schema designed by Dutch Kadaster, adding the spatial capabilities of the geodatabase and supporting the overall workflow of the data collection to be executed during the pilot.
The schema is implemented in the ArcGIS Workspace via a single feature class and two primary tables. The feature class SpatialUnit, with polygon geometry, represents the land units to be surveyed. Each of the features in the SpatialUnit feature class can have 0-n owners represented in Party table and associated via a 1-M relate to the feature class. In turn, each Party record can have 0-n rights to the feature in the SpatialUnit feature class as represented in the Right table via 1:M relate to the Party record. Because of the 1:M relates from SpatialUnit to Party to Right, each Record can be associated with one and only one Party record which in turn can be associated with one and only one SpatialUnit. Thus a surveyed property can have multiple owners each with percent shares of one or more rights to said property. The Right table records each owner‘s rights by types and percent shares of rights to the property.
 
The SpatialUnit, Party and Right database objects were also extended to allow attachments of additional files which could be virtually any digital file such as digital images or scanned documents.
Each of these database objects also has editor tracking enabled within the ArcGIS environment to record the user ID of the user who enters each SpatialUnit, Party or Right and the date and time of creation. Similarly whenever a SpatialUnit, Party or Right is subsequently edited, the user ID of the last editor is recorded along with the date and time of the edit.
The original schema included three elements which were omitted from the ArcGIS Workspace: 
  •	Administrative::La_AdministrativeSource
  •	External::ExtLandUse
  •	Administrative::LA_AdministrativeSourceType
These can be added if needed.
 

SpatialUnit:
The tables CO_Parcel and CO_SpatialSource were combined into the SpatialUnit feature class in the ArcGIS Workspace. I omitted the following fields since they seemed redundant with the spatial geometry of the geodatabase feature: 
  •	LA_SpatialUnit:area
  •	LA_SpatialUnit:dimension
  •	LA_SpatialUnit:referencePoint
  •	LA_SpatialUnit:surfaceRelation 
  •	LA_SpatialUnit:volume 
  •	LA_SpatialSource:measurements
  I omitted the following fields since they were redundant with the OBJECTID or GlobalID fields:
  •	LA_SpatialUnit:suID
  •	LA_Source:sID
 I omitted the following fields because I was simply uncertain of their role in the workflow or how to implement them in the ArcGIS Workspace. These can be added if they are necessary and we can determine their proper implementation in the geodatabase:
  •	LA_SpatialSource:procedure
  •	LA_Source:acceptance 
  •	LA_Source:availabilityStatus
  •	LA_Source:extArchiveID
  •	LA_Source:lifeSpanStamp 
  •	LA_Source:maintype 
  •	LA_Source:quality
  •	LA_Source:recordation
  •	LA_Source:sID 
  •	LA_Source:source
  •	LA_Source:submission
The SpatialUnits featureclass contains polygon features and combines certain elements of the CO_Parcels CO_SpatialSource tables. The LA_SpatialUnit:extAddressID field from CO_Parcel is implemented via a 1:M relate to the Addresses table in the ArcGIS Workspace. The LA_SpatialSource:type field from the CO_SpatialSource table is implemented as a coded value domain presenting the values fieldSketch, gnssSurvey, orthoPhoto, relativeMeasurement, topoMap, video.
The SpatialUnit feature class also can host attachments via the 1:M relate to the SpatialUnit_ATTACH table. Attachments might be photos of the property, scanned legal documents, plot maps, PDFs or virtually any other digital files. 
Party:
The CO_Party table, is implemented in the ArcGIS Workspace as the Party table, in a 1:M relation to the SpatialUnits feature class. I omitted the following fields since they were redundant with the OBJECTID or GlobalID fields:
  •	LA_Party:extPID
  •	LA_Party:pID
The LA_Party:type field from CO_Party is implemented in the Party:type field as a coded value domain with the following elements: 
  •	naturalPerson
  •	nonNaturalPerson
  •	group
  •	baunit.
The LA_Party:role field from CO_Party is implemented in the PartyRole table via a 1:M relation to the Party table. The PartyRole table contains the field PartyRole:role implemented as the LA_PartyRole coded value domain with the following elements: 
  •	bank
  •	certifiedSurveyor
  •	citizen
  •	conveyancer
  •	employee
  •	farmer
  •	moneyProvider
  •	notary
  •	stateAdministrator
  •	surveyor
  •	writer
The Party table also can host attachments via the 1:M relate to the Party_ATTACH table. Attachments might be photos of the person, scanned ID documents, PDFs or virtually any other digital files.
Right:
The CO_Right table is implemented in the ArcGIS Workspace as the Right table in a 1:M relation to the Party table. I omitted the LA_RRR:rID field since it was redundant with the OBJECTID or GlobalID fields.
The LA_Right:type field from the CO_Right table is implemented in the ArcGIS Workspace via the Right:type field as the LA_RightType coded value domain with the following elements: agriActivity, commonOwnership, customaryType, fireWood, fishing, grazing, informalOccupation, lease, occupation, ownership, ownershipAssumed, superficies, usufruct, waterrights, tenancy.
The LA_RRR:share field from the CO_Right table is implemented in the ArcGIS Workspace via the Right:share field as the LA_Percent range domain constraining the valid input values to a range from 0.00 to 100.00. 
The LA_RRR:shareCheck field from the CO_Right table is implemented in the ArcGIS Workspace via the Right:shareCheck field as the LA_YesNo coded value domain constraining the valid input values to either 0 (No/False) or 1 (Yes/True). 
I omitted the LA_RRR:timeSpec field because I could not find documentation as to its type or purpose. It can be added if needed.
The Right table also can host attachments via the 1:M relate to the Right_ATTACH table. Attachments might be scanned legal documents, PDFs or virtually any other digital files.
