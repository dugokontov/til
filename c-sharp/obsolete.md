## Obsolete (depricated)

[ObsoleteAttribute] is an attribute used to denote a method that will soon be put out of the use (depricated).

### Why?

If we are planing to roll out new version, but still have to support old one for some time, and we want to leave a useful comment to future developers that this method could be removed after some condition is met.

### How?

Just add attribute above the method that will be depricated. 

```c#
[Obsolete("This is the old way to get records. When we move all projects to use new method, this one can be deleted")]
public int GetNumberOfRecords()
{
  // actual code
}
```

