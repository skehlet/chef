<!---
This file is reset every time a new release is done. This file describes changes that have not yet been released.

Example Doc Change:
### Headline for the required change
Description of the required change.
-->

### Resources must now use `provides` to declare recipe DSL

Resources declared in `Chef::Resource` namespace will no longer get recipe DSL
automatically.  Instead, explicit `provides` is required in order to have DSL:

```ruby
module MyModule
  class MyResource < Chef::Resource
    provides :my_resource
  end
end
```

Authors of HWRPs need to be aware that in the future all resources and providers will be required to include a provides line. Starting with Chef 12.4.0 any HWRPs in the `Chef::Resource` or `Chef::Provider` namespaces that do not have provides lines will trigger deprecation warning messages when called. The LWRPBase code does `provides` automatically so LWRP authors and users who write classes that inherit from LWRPBase do not need to explicitly include provides lines.

Users are encouraged to declare resources in their own namespaces instead of putting them in the special `Chef::Resource` namespace.

### LWRPs are no longer automatically placed in the `Chef::Resource` namespace

Starting with Chef 12.4.0, accessing an LWRP class by name from the `Chef::Resource` namespace will trigger a deprecation warning message. This means that if your cookbook includes the LWRP `mycookbook/resources/myresource.rb`, you will no longer be able to extend or reference `Chef::Resource::MycookbookMyresource` in Ruby code.  LWRP recipe DSL does not change: the LWRP will still be available to recipes as `mycookbook_myresource`.

You can still get the LWRP class by calling `Chef::ResourceResolver.resolve(:mycookbook_myresource)`.

The primary aim here is clearing out the `Chef::Resource` namespace.

References to these classes is deprecated (and will emit a warning) in Chef 12, and will be removed in Chef 13.
