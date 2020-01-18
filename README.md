# Tagger.Reflect

.NET library to mock object properties with attributes.

## What is for?

I initially designed this library for writing tests for [Command Line Parser Library](https://github.com/commandlineparser/commandline). Although it's main purpose is testing, it can be helpful in the scope of dynamic programming.

## Targets

- .NET Standard 2.0
- .NET Framework 4.5, 4.6.1

## Install via NuGet

```sh
$ dotnet add package Tagger.Reflect --version 1.0.2
```

## At a glance

```csharp
var sut = new Mirror(new {
	StringProperty = default(string), IntProperty = default(int), BooleanProperty = default(bool) })
  .Implement<IMyInterface>()
  .AddAttribute<MyAttribute>(
    "StringProperty",
    new AttributeConfiguration()
      .CtorValue("ctor value")
      .Property("AttributeProperty", "property value"));

var instance = sut.Unwrap<IMyInterface>();
```

## Usage

```csharp
public class AddPropertyMethod
{
    [Theory]
    [InlineData("DynamicString", typeof(string))]
    [InlineData("DynamicBoolean", typeof(bool))]
    [InlineData("DynamicLong", typeof(long))]
    public void Add_property_to_new_object_returns_object_with_new_property(string propertyName, Type propertyType)
    {
        var sut = new Mirror().AddProperty(propertyName, propertyType);

        sut.Object.GetType().GetProperties().Should().Contain(
            p => p.Name.Equals(propertyName) && p.PropertyType == propertyType);
    }
}
```
See this [unit test](https://github.com/gsscoder/tagger/blob/master/tests/Tagger.Reflect.Tests/Unit/MirrorTests.cs) for more examples.

## Latest Changes

- Ported to .NET Core.
- Compiles for .NET Framework 4.5 and 4.6.1.
- Name changed to Tagger.Reflect