# Notes on gRPC

# Proto Buffs:

a. Protocol Buffers are a language-neutral, platform-neutral extensible mechanism for serializing structured data.

b. Define how you want your data to be structured once, then you can use special generated source code to easily write and read your structured data to and from a variety of data streams and using a variety of languages.

c. Protocol buffers are a combination of the definition language (created in .proto files), the code that the proto compiler generates to interface with data, language-specific runtime libraries, and the serialization format for data that is written to a file (or sent across a network connection).

d. The proto compiler is invoked at build time on .proto files to generate code in various programming languages

e. Each generated class contains simple accessors for each field and methods to serialize and parse the whole structure to and from raw bytes.

f. Protocol buffers allow for the seamless support of changes, including the addition of new fields and the deletion of existing fields, to any protocol buffer without breaking existing services.

g. When updating .proto definitions, old code will read new messages without issues, ignoring any newly added fields. To the old code, fields that were deleted will have their default value, and deleted repeated fields will be empty.

h. New code will also transparently read old messages. New fields will not be present in old messages; in these cases protocol buffers provide a reasonable default value.

i. A field can also be of:

        - A message type, so that you can nest parts of the definition, such as for repeating sets of data.
        - An enum type, so you can specify a set of values to choose from.
        - A oneof type, which you can use when a message has many optional fields and at most one field will be set at the same time.
        - A map type, to add key-value pairs to your definition.

j. Protos have fixed type, they are binary so easy to transmit, they support different language.

k. When buikld, these proto files generate language specifix files from the proto files. Hence one proto file can be used in different languages.

l. .proto file => Generate code in different langugages => Serialize them to binary. This motivates communication between servers in diff languages

m. Proto buff are used by gRPC. gRPC is mainly used for server communication. The request and response is via protoBuffs.

n. For proto buff, we need to define the default values, else if we don't mention the value in payload, then the field will not be considered only. If we define the default value, and if we don't mention that fiels in payload, it will take the default value we define.
This is why point "f " is true.

o. Number formats in proto: int32, int64, sint32, sint64, uint32, uint64, fixed32, fixed64, sfixed32, sfixed 64, double, float.
For all above types default value can be 0.
sint32 can take neg numbers while unint32 will take only positive numbers.

p. For boolean => Key:bool; value: true/false; default: false.

q. For string => Key: string; value:arbitary Length of text; Default: empty string.

r. Bytes are also a type.

Imp\*\* repeated is a type to be used for fields that can have 0 or more values and is not fixed.Default value is empty array[] Example:
message MyMessage {
repeated int32 numbers = 1;
repeated string names = 2;
}

here numbers can have any number of values like[3],[],[2 7 5]

s. Enums are also a type in proto, the first tag in enum should be 1.

enum Color{
RED=0;
BLUE=1;
GREEN=3;
}

message Flag{
string country=1;
Color flagColor=2
}

t. message MyMessage {
int32 field1 = 1;
string field2 = 2;
bool field3 = 3;
}

Here we are defining numbers 1,2,3,... as a unique id for each field within a message. They are also called tags.. Must fields in proto are tagged usually between 1-15 then 16-2047...and so on. Min value=1, max is around 56M.

u. 1900-1999 are reserved by google.

v. In order to perform code generation, you will need to install protoc on your computer.