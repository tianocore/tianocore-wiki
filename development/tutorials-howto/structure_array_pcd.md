# Structure Array Pcd

## PCD can be declared as C Array in DEC file

* When PCD is declared in DEC file, VOID* will be replaced by C Array.
* C Array can be basic data type array, such as `UINT8[10]`
* C Array can be structure array, such as `TEST[20]`
* C Array can be flexible array, such as `TEST[]`
`gTestPkgTokenSpaceGuid.PcdName|{0x00}|TEST[]|0x10000000`
* C Array PCD value is VOID * type. C source code uses `PcdGetPtr(PcdName)` API
to get PCD value pointer, then convert it to C array, and access the element value.
* If PCD is configured as Dynamic/DynamicEx PCD, C source code can use
`PcdSetPtrS(PcdName, SizeOfBuffer, Buffer)` API to update PCD value.

## Structure or Array PCD value as C style initialization in DEC/DSC

* Globle C structure or array can be initialized with the value.
* If PCD is declared as C Structure or Array, its value can be specified with C style value.
For example:
`gTestPkgTokenSpaceGuid.PcdName|{CODE({`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`{0, {0, 0, 0, 0,  0, 0, 0}},`
`})}`
