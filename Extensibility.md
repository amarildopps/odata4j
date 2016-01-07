# Client-side: OClientBehavior #

### Request customization ###
_TODO_

### Jersey client customization ###
You have the option of modifying the Jersey ClientConfig (adding standard jaxrs providers/properties) inside a custom behavior.  These modifications will apply to all requests issued by the consumer.

#### Example 1 ####
Insert `<category />` element into atom creation requests. [RequestEntryModificationExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-core/src/test/java/org/odata4j/examples/consumer/RequestEntryModificationExample.java)

```java

public class RequestEntryModificationExample {

public static void main(String... args) {
ODataConsumer.dump.all(true);

// create a consumer with additional behavior
String serviceUri = "http://services.odata.org/Northwind/Northwind.svc";
final ModifiableAtomEntryMessageBodyWriter writer = new ModifiableAtomEntryMessageBodyWriter();
ODataConsumer consumer = ODataConsumer.newBuilder(serviceUri).setClientBehaviors(new BaseClientBehavior(){
@Override
public void modify(ClientConfig cc) {
cc.getSingletons().add(writer);
}}).build();

// set category for subsequent entry creation requests
writer.setEntryXmlModification(insertCategory("NorthwindModel.Categories"));
consumer.createEntity("Categories")
.properties(OProperties.string("CategoryName", "Category "+new Date()))
.execute();
}

private static Func1<String, String> insertCategory(final String term) {
return new Func1<String, String>(){
@Override
public String apply(String entryXml) {
int i = entryXml.indexOf("

Unknown end tag for &lt;/author&gt;

<content");
if (i < 0)
return entryXml;
i += "

Unknown end tag for &lt;/author&gt;

".length();
String categoryXml = String.format("<category term=\"%s\" scheme=\"http://schemas.microsoft.com/ado/2007/08/dataservices/scheme\"/>", term);
return entryXml.substring(0, i) + categoryXml + entryXml.substring(i);
}};
}

@Produces(ODataConstants.APPLICATION_ATOM_XML_CHARSET_UTF8)
private static class ModifiableAtomEntryMessageBodyWriter implements MessageBodyWriter<String> {

private Func1<String, String> entryXmlModification;

public void setEntryXmlModification(Func1<String, String> entryXmlModification) {
this.entryXmlModification = entryXmlModification;
}

@Override
public boolean isWriteable(Class<?> type, Type genericType, Annotation[] annotations, MediaType mediaType) {
return type == String.class;
}

@Override
public long getSize(String entryXml, Class<?> type, Type genericType, Annotation[] annotations, MediaType mediaType) {
return -1L;
}


@Override
public void writeTo(String entryXml, Class<?> type, Type genericType, Annotation[] annotations, MediaType mediaType,
MultivaluedMap<String, Object> httpHeaders, OutputStream entityStream)
throws IOException, WebApplicationException {
if (entryXmlModification != null)
entryXml = entryXmlModification.apply(entryXml);
ReaderWriter.writeToAsString(entryXml, entityStream, mediaType);
}

}

}
```

#### Example 2 ####
Grab the raw json response from the underlying request. [JsonGrabbingConsumerExample.java](http://code.google.com/p/odata4j/source/browse/odata4j-core/src/test/java/org/odata4j/examples/consumer/JsonGrabbingConsumerExample.java)

```java

public class JsonGrabbingConsumerExample {

public static void main(String[] args) {
ResponseGrabbingClientBehavior responseGrabbingBehavior = new ResponseGrabbingClientBehavior();

String serviceUri = "http://services.odata.org/Northwind/Northwind.svc";
ODataConsumer c = ODataConsumer.newBuilder(serviceUri)
.setFormatType(FormatType.JSON)
.setClientBehaviors(responseGrabbingBehavior)
.build();

c.getEntity("Customers", "VICTE").execute();
System.out.println(responseGrabbingBehavior.lastResponse);
}

private static class ResponseGrabbingClientBehavior extends BaseClientBehavior {

public String lastResponse;

@Override
public void modifyClientFilters(Filterable client) {
client.addFilter(new ClientFilter(){
@Override
public ClientResponse handle(ClientRequest clientRequest) throws ClientHandlerException {
ClientResponse response = getNext().handle(clientRequest);
lastResponse = response.getEntity(String.class);
// we consumed the response stream, replace it to avoid breaking downstream processing
response.setEntityInputStream(new ByteArrayInputStream(lastResponse.getBytes()));
return response;
}});
}

}

}
```