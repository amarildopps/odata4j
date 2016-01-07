# Introduction #

Queryable metadata was introduced in a [blog post](http://www.odata.org/blog/2010/4/22/queryable-odata-metadata) on odata.org.  The notion of querying individual bits of metadata instead of the monolithic $metadata document is a fantastic idea and a feature that we need now.

This Wiki entry details a prototype implementation of queryable metadata in odata4j.  This implementation is in the spirit of the blog post but departs from how the metadata is modeled.  (initial commit revision: [bd5d30509c35](http://code.google.com/p/odata4j/source/detail?r=bd5d30509c354e80b2d9f9277c3a45c7283565de) date: 2011-09-27 13:40:04 -0600)


# Model of the Model #

The bits of metadata that will be exposed, such as Schemas, EntityTypes, EntitySets, etc are all easily modeled using the EDM itself and this is the approach that has been taken here.  First, lets peek at the service document.  I'm using the stock InMemoryProducerExample.  To get the service document we use the regular $metadata URL with an **Accept** header specifying **application/atomsvc+xml**.  As a convenience, the query parameter **$format=atomsvc** will also work:

```
http://localhost:8887/InMemoryProducerExample.svc/$metadata?$format=atomsvc
```

```
<?xml version="1.0" encoding="utf-8"?>
<service xmlns="http://www.w3.org/2007/app" xml:base="http://localhost:8887/InMemoryProducerExample.svc/" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:app="http://www.w3.org/2007/app">
    <workspace>
        <atom:title>Default</atom:title>
        <collection href="Schemas">
            <atom:title>Schemas</atom:title>
        </collection>
        <collection href="ComplexTypes">
            <atom:title>ComplexTypes</atom:title>
        </collection>
        <collection href="RootComplexTypes">
            <atom:title>RootComplexTypes</atom:title>
        </collection>
        <collection href="EntityTypes">
            <atom:title>EntityTypes</atom:title>
        </collection>
        <collection href="RootEntityTypes">
            <atom:title>RootEntityTypes</atom:title>
        </collection>
        <collection href="Properties">
            <atom:title>Properties</atom:title>
        </collection>
    </workspace>
</service>
```

The metamodel of the EDM is expressed like any other model in the EDM  Schemas is an EntitySet just like Customers or Orders.  See the Extensions section for the reason behind Root[Entity|Complex]Types.  One accesses the metadata collections by giving the entity set name after $metadata on the URL.  Lets look at the available schemas:

```
http://localhost:8887/InMemoryProducerExample.svc/$metadata/Schemas?$format=json
```

```
{
    "d" : {
        "results" : [
        {
            "__metadata" : {
                "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Schemas('InMemoryProducerExample')", 
                "type" : "com.microsoft.schemas.ado._2008._09.edm.Schema"
            }, 
            "Namespace" : "InMemoryProducerExample", 
            "inmem_Version" : "1.0 early experience pre-alpha", 
            "inmem_SchemaInfo" : {
                "Author" : "Xavier S. Dumont", 
                "SeeAlso" : "InMemoryProducerExample.java"
            }, 
            "ComplexTypes" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Schemas('InMemoryProducerExample')/ComplexTypes"
                }
            }, 
            "EntityTypes" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Schemas('InMemoryProducerExample')/EntityTypes"
                }
            }
        }
        ]
    }
}
```

A Schema entity looks just like a Schema element in the normative CSDL documentation.  Note the two properties named with a leading "inmem_".  These are AnnotationAttributes and AnnotationElements.  More on that later._


Of course, one can also access the meta model to see what a Schema entity is composed of:
```
http://localhost:8887/InMemoryProducerExample.svc/$metadata?$format=metamodel
```
```
            <EntityType Name="Schema">
                <Key>
                    <PropertyRef Name="Namespace"></PropertyRef>
                </Key>
                <Property Name="Namespace" Type="Edm.String" Nullable="false"></Property>
                <Property Name="Alias" Type="Edm.String" Nullable="true"></Property>
                <NavigationProperty Name="EntityTypes" Relationship="com.microsoft.schemas.ado._2008._09.edm.EntityTypes" FromRole="Schema" ToRole="StructuralType"></NavigationProperty>
                <NavigationProperty Name="ComplexTypes" Relationship="com.microsoft.schemas.ado._2008._09.edm.ComplexTypes" FromRole="Schema" ToRole="ComplexType"></NavigationProperty>
            </EntityType>
```

This is the MetaModel of what has been implemented so far.

# Query the Model #

The true power of this approach is realized when one starts writing $filters and using $select and $expand.  Example:

```
http://localhost:8887/InMemoryProducerExample.svc/$metadata/EntityTypes?$format=json&$filter=Name eq 'ETFs'

(many $filter functions are not implemented...yet.  You can however filter on AnnotationAttribute values!)
```
```
{
    "d" : {
        "results" : [
        {
            "__metadata" : {
                "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')", 
                "type" : "com.microsoft.schemas.ado._2008._09.edm.EntityType"
            }, 
            "Name" : "ETFs", 
            "Namespace" : "InMemoryProducerExample", 
            "Key" : {
                "Keys" : {
                    "results" : [
                    {
                        "Name" : "EntityId"
                    }
                    ]
                }
            }, 
            "Properties" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/Properties"
                }
            }, 
            "SuperType" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/SuperType"
                }
            }, 
            "SubTypes" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/SubTypes"
                }
            }
        }
        ]
    }
}
```

Need the properties of an ETF?  Just $expand them:
```
http://localhost:8887/InMemoryProducerExample.svc/$metadata/EntityTypes?$format=json&$filter=Name eq 'ETFs'&$expand=Properties
```
```
{
    "d" : {
        "results" : [
        {
            "__metadata" : {
                "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')", 
                "type" : "com.microsoft.schemas.ado._2008._09.edm.EntityType"
            }, 
            "Name" : "ETFs", 
            "Namespace" : "InMemoryProducerExample", 
            "Key" : {
                "Keys" : {
                    "results" : [
                    {
                        "Name" : "EntityId"
                    }
                    ]
                }
            }, 
            "Properties" : {
                "results" : [
                {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='EntityId',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Namespace" : "InMemoryProducerExample", 
                    "EntityTypeName" : "ETFs", 
                    "Name" : "EntityId", 
                    "Type" : "Edm.String", 
                    "Nullable" : true
                }, {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='Name',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Namespace" : "InMemoryProducerExample", 
                    "EntityTypeName" : "ETFs", 
                    "Name" : "Name", 
                    "Type" : "Edm.String", 
                    "Nullable" : true
                }, {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='FundType',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Namespace" : "InMemoryProducerExample", 
                    "EntityTypeName" : "ETFs", 
                    "Name" : "FundType", 
                    "Type" : "Edm.String", 
                    "Nullable" : true
                }, {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='Symbol',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Namespace" : "InMemoryProducerExample", 
                    "EntityTypeName" : "ETFs", 
                    "Name" : "Symbol", 
                    "Type" : "Edm.String", 
                    "Nullable" : true
                }
                ]
            }, 
            "SuperType" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/SuperType"
                }
            }, 
            "SubTypes" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/SubTypes"
                }
            }
        }
        ]
    }
}
```
Just care about the Property Names?  $select works also:
```
http://localhost:8887/InMemoryProducerExample.svc/$metadata/EntityTypes?$format=json&$filter=Name eq 'ETFs'&$expand=Properties&$select=Properties/Name
```
```
{
    "d" : {
        "results" : [
        {
            "__metadata" : {
                "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')", 
                "type" : "com.microsoft.schemas.ado._2008._09.edm.EntityType"
            }, 
            "Name" : "ETFs", 
            "Namespace" : "InMemoryProducerExample", 
            "Key" : {
                "Keys" : {
                    "results" : [
                    {
                        "Name" : "EntityId"
                    }
                    ]
                }
            }, 
            "Properties" : {
                "results" : [
                {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='EntityId',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Name" : "EntityId"
                }, {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='Name',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Name" : "Name"
                }, {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='FundType',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Name" : "FundType"
                }, {
                    "__metadata" : {
                        "uri" : "http://localhost:8887/InMemoryProducerExample.svc/Properties(EntityTypeName='ETFs',Name='Symbol',Namespace='InMemoryProducerExample')", 
                        "type" : "com.microsoft.schemas.ado._2008._09.edm.Property"
                    }, 
                    "Name" : "Symbol"
                }
                ]
            }, 
            "SuperType" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/SuperType"
                }
            }, 
            "SubTypes" : {
                "__deferred" : {
                    "uri" : "http://localhost:8887/InMemoryProducerExample.svc/EntityTypes(Name='ETFs',Namespace='InMemoryProducerExample')/SubTypes"
                }
            }
        }
        ]
    }
}
```

# Implementation #
The good news is that the bulk of the queryable metadata production lives in the odata4j framework.  A single new method on ODataProducer is all you need to implement to get the ball rolling:

```
MetadataProducer getMetadataProducer();
```

MetadataProducer is a framework class that implements ODataProducer and gets wired into the MetadataResource.  All application specific annotatations, documentation, and other hooks/extensions are achieved by giving your MetadataProducer an object that implements a new interface called EdmDecorator.

### Annotations and Documentation ###
The InMemoryProducerExample.java shows how to use EdmDecorator to get Documentation and AnnotationElements and AnnotationAttributes attached to your model by implementing methods on EdmDecorator:

```
    @Override
    public EdmDocumentation getDocumentationForSchema(String namespace, String typeName) {
      return new EdmDocumentation("InMemoryProducerExample", "This schema exposes a few example types to demonstrate the InMemoryProducer");
    }

   @Override
    public List<EdmAnnotation> getAnnotationsForSchema(String namespace, String typeName) {
      List<EdmAnnotation> annots = new ArrayList<EdmAnnotation>();
      annots.add(new EdmAnnotationAttribute(namespace, prefix, "Version", "1.0 early experience pre-alpha"));
      
      List<OProperty<?>> p = new ArrayList<OProperty<?>>();
      p.add(OProperties.string("Author", "Xavier S. Dumont"));
      p.add(OProperties.string("SeeAlso", "InMemoryProducerExample.java"));
      
      annots.add(new EdmAnnotationElement(namespace, prefix, "SchemaInfo", OComplexObjects.create(schemaInfoType, p)));

      return annots;
    }

```

**A note on Annotation names, xml schemas and prefixes**
```
/*
 * property naming: so...annotations live in a namespace.  JSON doesn't have the concept of namespaces,
 * I think <prefix>_<propname> makes the most sense.  We *could* use <prefix>:<propname> if we quoted the
 * json key..that isn't a universally supported thing though.
 * The issue gets weird with Atom.  The OData spec says that each sub-element of <m:properties> must live
 * in the data service namespace....I guess I'll just use the same JSON name....this of course makes
  * the queryable metadata property names different than the names one would see from $metadata...not
  * sure we can do anything about that.
  */
```
What do you think?

## Extensions and Enhancements ##
Our custom producer exposes a large class hierarchy.  Our clients are driven heavily by metadata and are often interested in a limited subset of the hierarchy at a time.  These needs drove three features:  localization support, recursive select/expand, and type flattening.

### Localization ###
Each EntityType and Property in our model includes a localized label that we attach as an AnnotationAttribute.  Two things were added to support localizing these labels.  First, MetadataProducer will look for a **custom query parameter called 'locale'** whose value should be a locale string ala java.util.Locale.  Second, EdmDecorator provides a hook that is called as the MetadataProducer constructs OEntities to return:
```
/**
   * Get an annotation value that overrides the original annotation value.
   * This is an experiment that allows one to localize queryable metadata.
   * Say you have an annotation called LocalizedName on your item.  When 
   * the metadata is queried, the caller can supply a custom locale parameter
   * in options and this method can override the original LocalizedName with
   * the one for the given locale.
   * @param item - the annotated item.
   * @param annot - the annotation
   * @param options - from query
   * @return - null if no override, an object with the value if there is one.
   */
public Object getAnnotationValueOverride(EdmItem item, Annotation<?> annot, boolean flatten, Locale locale, Map<String, String> options)
```

### Type flattening ###
The default behavior of MetadataProducer when querying a StructuralType and $expanding the Properties navigation property is to include only those properties declared on that type and not include any inherited properties.  However, clients typically need all of the properties in the lineage.  As a convenience, a **custom query parameter 'flatten'** will cause the producer to include all properties in the inheritance lineage for StructuralTypes.  See 'Recursive expand' for an alternative.

### Recursive expand and select ###
OData is good at traversing simple object structures but lacks support for graphs which is what an inheritance hierarchy is (obviously).  This implementation includes such support.  Here is the comment from the code:
```
Two new custom options are proposed:
 * 
 * expandR and selectR
 * 
 * ABNF:
 * expandRQueryOp = "expandR=" recursiveExpandClause *("," recursiveExpandClause)
 * recursiveExpandClause = entityNavProperty "/" expandDepth
 * expandDepth = integer
 * 
 * selectRQueryOp = "selectR=" recursiveSelectClause *("," recursiveSelectClause)
 * recursiveSelectClause = rSelectItem *("," recursiveSelectClause)
 * rSelectItem = selectedNavProperty "/" rPropItem
 * rPropItem = "*" / selectedProperty
 * 
 * expandDepth drives the number of traversal iterations.  An expandDepth of 0 is
 * unlimited.  During query processing, the max expandDepth of all recursivExpandClauses
 * is computed and drives processing.
 * 
 * example:
 *  expandR=SubTypes/0,Properties/1 
 *  
 * This says that at each position in the object graph traversal
 *  during query we will expand the SubTypes navigation property.  At the first
 *  level we will also expand the Properties navigation property
 * 
 *  selectR=SubTypes/Namespace,SubTypes/Type
 * 
 *  This says that whenever we expand the SubTypes navigation property we will only 
 *  include Namespace and Type properties.
```

This allows one to query up and down the hierarchy.  Down:

```
/$metadata/RootEntityTypes(Namespace="ns",Name="MyBaseClass")?expandR=SubTypes/0
```

Up:
```
/$metadata/EntityTypes(Namespace="ns",Name="MyLeafClass")?expandR=SuperType/0
```

# Finally #
Let the discussion begin...if everyone likes this perhaps we can convince Microsoft to implement it in .NET :)