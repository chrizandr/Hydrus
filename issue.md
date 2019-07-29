### I'm submitting a 
- [ ] bug report.
- [x] feature request.

### Current Behaviour:
<!-- Describe about the bug -->
I've been speculating for some time now that our treatment of collections as a single resource for every class is wrong. Nowhere in the spec is it mentioned that a collection is a set of all objects for a given class. It is mentioned that a collection is "a set of somehow related resources". This does not imply that a collection would be only limited to class types
### Expected Behaviour:
<!-- Describe what will happen if bug is removed -->
 I think the direction we must move in is for users to be able to define collections on their own. Each collection itself would have an `@id` since it is a subclass of `hydra:Resource`. We then use the collection endpoint to relate classes/properties. 

We use Collections directly instead of having pseudo-Collections defined by parent_id or child_id. The main problem with both the above approaches is as you mentioned, identifying the property which contains the parent/child id. This is because schema.org/identifier or hydra:Link and even https://schema.org/ItemList are general definitions that can be used for any property and need not necessarily be for only parent/child ids. The advantage of using a collection is that we define a CommentCollection object, where we link all Comment objects for a particular issue. We then give that CommentCollection object as the property for the Issue. In terms of defining it in the API documentation, it would be something like:

```
{
        "@type": "SupportedProperty",
        "property": vocab:CommentCollection,
        "readonly": "false",
        "required": "true",
        "title": "Comments",
        "writeonly": "false"
}
```
The property would map to an instance of the CommanCollection class. This will be defined in the supportedProperty field for the definition of the Issue class. Like any other object, when we define the Issue object, we will add the appropriate instance of the CommandCollection to the Issue instance. So an object would be something like:

```
{
        "@type": "Issue",
        "@id": "/api/Issues/27",
        "Issue": "....",
        "Comments": "/api/CommentCollection/32",
        ....
}
```

And then `/api/CommentCollection/32` would have:

```
{
        "@type": "CommentCollection",
        "@id": "/api/CommentCollection/32",
        "members": [
                {
                        "@id": "/api/Comment/12",
                        "@type": "Comment"
                },
                {
                        "@id": "/api/Comment/18",
                        "@type": "Comment"
                },
                {
                        "@id": "/api/Comment/22",
                        "@type": "Comment"
                }
        ]

}
```



