## Support special relative URI notation when Unicode is present

### Scope
Edge

### Version Introduced
4.7.3

### Source Analyzer Status
NotPlanned

### Change Description

System.Uri will no longer throw a NullReferenceException when calling TryCreate on unusual relative URIs containing Unicode.

The simplest reproduction of the NRE is below, with the two statements being equivalent:
 ```C#
bool success = Uri.TryCreate("http:%C3%A8", UriKind.RelativeOrAbsolute, out Uri href);
bool success = Uri.TryCreate("http:è", UriKind.RelativeOrAbsolute, out Uri href);
 ```
To reproduce the issue the two following items must be true:
- The URI must be specified as relative by prepending it with ‘http:’ but no ‘//’.
- The URI must contain percent encoded Unicode or unreserved symbols.

- [ ] Quirked // Uses some mechanism to turn the feature on or off, usually using runtime targeting, AppContext or config files. Needs to be turned on automatically for some situations.
- [ ] Build-time break // Causes a break if attempted to recompile

### Recommended Action
Users depending on this behavior to disallow relative URIs should instead specify `T:System.UriKind.Absolute`.

### Affected APIs
* `T:System.Uri.TryCreate`

### Category
Core

<!--
    ### Original Bug
    https://devdiv.visualstudio.com/DevDiv/_workitems/edit/287019
-->