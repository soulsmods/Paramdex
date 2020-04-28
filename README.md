# Paramdex
A collection of resources for working with params via [SoulsFormats](https://github.com/JKAnderson/SoulsFormats), [Yapped](https://github.com/JKAnderson/Yapped), etc.  
Three folders are included for each game:  
* **Defs** - paramdefs required to edit param data, given in XML format. Filenames are arbitrary; apply based on the ParamType after loading.
* **Names** - predefined row names, primarily for newer games with names stripped. Names should not be assumed to be present for every param.
* **Tdfs** - paramtdfs provide friendly names for enumerated types. Only present for Demon's Souls until further notice.

Paramdefs for DeS, DS1, and BB were shipped with the games and should not be altered beyond translating display names and descriptions.  
Paramdefs for other games were reverse-engineered, so improvements are welcome.  

Game | Directory
-----|----------
Dark Souls | DS1
Dark Souls: Remastered | DS1R
Dark Souls II: Scholar of the First Sin | DS2S
Dark Souls III | DS3
Demon's Souls | DES
Bloodborne | BB
Sekiro | SDT

## Writing Paramdefs
All paramdefs should contain the following elements:  

Element | Description
--------|------------
ParamType | Correlates the paramdef to its params
DataVersion | Indicates a revision of the param data structure
BigEndian | Whether the binary format is big-endian
Unicode | Whether description strings are encoded in UTF-16
FormatVersion | Determines the binary format of the paramdef
Fields | Contains the Field elements that describe param data

Each Field element has a mandatory Def attribute that specifies the type, internal name, bit size or array length if needed, and (optionally) default value. Bit size and array length are written as in a C declaration.  
```xml
<!-- Minimum required info -->
<Field Def="u16 iconId" />
<!-- Specifying a default value -->
<Field Def="u16 iconId = 1000" />
<!-- Defining a bitfield -->
<Field Def="u8 enableGuard:1" />
<!-- Defining an array -->
<Field Def="dummy8 padding[2]" />
```
Additionally, the following elements may be added to customize editor functionality:  

Element | Description
--------|------------
DisplayName | A more human-readable name for the field
Enum | Indicates an enumerated type; corresponds to a paramtdf
Description | A description of the field's purpose
DisplayFormat | A printf format string for displaying the value
EditFlags | Controls editor behavior; known flags are Wrap and Lock
Minimum | The minimum acceptable value
Maximum | The maximum acceptable value
Increment | How much to increase or decrease the value per step
SortID | An arbitrary number that determines display ordering of fields

Any elements not provided will be given sensible default values based on the field's type. Default, Minimum, Maximum, and Increment are always specified as floats, so they are not meaningful for array types.  
```xml
<!-- A fully-loaded declaration -->
<Field Def="u32 foobarType:7 = 15">
  <DisplayName>Foobar Type</DisplayName>
  <Enum>FOOBAR_TYPE</Enum>
  <Description>Determines the foobarosity of the whizbang.</Description>
  <DisplayFormat>%8x</DisplayFormat>
  <EditFlags>Wrap</EditFlags>
  <Minimum>10</Minimum>
  <Maximum>100</Maximum>
  <Increment>5</Increment>
  <SortID>1500</SortID>
</Field>
```
All available types are listed below.  

Type | Supports Bit Size | Supports Array Length | Description
-----|-------------------|-----------------------|------------
s8 | No | No | One-byte signed int
u8 | Yes | No | One-byte unsigned int
s16 | No | No | Two-byte signed int
u16 | Yes | No | Two-byte unsigned int
s32 | No | No | Four-byte signed int
u32 | Yes | No | Four-byte unsigned int
f32 | No | No | Single-precision floating point
fixstr | No | Mandatory | Fixed-width Shift-JIS string
fixstrW | No | Mandatory | Fixed-width UTF-16 string
dummy8 | Yes | Yes | Byte or array of bytes used for padding

## Contributors
EvanDeadlySins  
GiveMeThePowa  
[katalash](https://github.com/katalash)  
[Pavuk](https://github.com/JohrnaJohrna)  
[thefifthmatt](https://github.com/thefifthmatt)  
[TKGP](https://github.com/JKAnderson)  
[Vawser](https://github.com/vawser)  
