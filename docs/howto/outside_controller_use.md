[Back to Guides](../README.md)

## Using ActiveModelSerializers Outside Of A Controller

### Serializing a resource

In ActiveModelSerializers versions 0.10 or later, serializing resources outside of the controller context is fairly simple:

```ruby
# Create our resource
post = Post.create(title: "Sample post", body: "I love Active Model Serializers!")

# Optional options parameters
options = {}

# Create a serializable resource instance
serializable_resource = ActiveModel::SerializableResource.new(post, options)

# Convert your resource into json
model_json = serializable_resource.as_json
```

### Looking up the Serializer for a Resource

If you want to retrieve a serializer for a specific resource, you can do the following:

```ruby
# Create our resource
post = Post.create(title: "Another Example", body: "So much fun.")

# Optional options parameters
options = {}

# Retrieve the default serializer for posts
serializer = ActiveModel::Serializer.serializer_for(post, options)
```

You could also retrieve the serializer via:

```ruby
ActiveModel::SerializableResource.new(post, options).serializer
```

Both approaches will return an instance, if any, of the resource's serializer.

## Serializing before controller render

At times, you might want to use a serializer without rendering it to the view. For those cases, you can create an instance of `ActiveModel::SerializableResource` with
the resource you want to be serialized and call `.as_json`.

```ruby
def create
  message = current_user.messages.create!(message_params)
  message_json = ActiveModel::SerializableResource.new(message).as_json
  MessageCreationWorker.perform(message_json)
  head 204
end
```
