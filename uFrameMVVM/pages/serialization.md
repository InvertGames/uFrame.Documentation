# Serialization

[Description]


## {ViewModel}.SerializeToJSON


To serialize any [Element](element-node.md) to JSON you can use the following in any [View](view-node.md)

    ViewModel::SerializeToJSON() : string
    ViewModel::DeserializeFromJSON(json : string) : void


### Change serialization

Certain types are not serialized by default by `SerializeToJSON()`.

These are:
- SimpleClass
- TypeReferences

You can modify what is serialized in your [ViewModel](view-models.md) if you overwrite the following functions.

	public void Write(ISerializerStream stream)
    public void Read(ISerializerStream stream)


## Known Issues

There is currently a bug with the `string` Type. You have to reselect `string` on your Properties in your Element so that those are serialized, too.

[Insert Images]

	
