**Extensibility**: 
Cobalt runtime may have clients that want to augment the information returned in this structure. We will push additional information to the "features" key, at both the "hypothesis" and "token" level, instead of altering the structure of this basic object.

**Backward Compatibility**: 
As much as possible, we look to not change the structure of this JSon, as we will be dealing with client code that may or may not be updated. 
