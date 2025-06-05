# Conversions
Converts a flat json into OSC-EM conform data. 

## Usage
The converter can take any flat json and be given an additional mapping table in form of .csv to convert to OSC-EM
The csv needs to follow a similar approach to the default included life sciences one, albeit at a reduced complexity:
It requires the following colummns:
- oscem: The oscem field thats mapped to; . seperated for nesting.
- fromformat: What the key is called in the input format json.
- optionals: If there are any optional namings that might map to the same field at an increased priority if present.
- units: The unit of any given field, if applicable.
- crunch: The conversion factor to arrive at your desired output unit, based on the value in the input json. 
- type: The type of the field, allowed values are: Int, String, Float64, Bool

When using the Converter as a standalone tool you can compile it using the cmd/convert_cli/ path then:

```sh
go build main.go
```

It accepts the following inputs:
-i input json
-o output filename (will take directory name if none provided, optional)
-map path to the mapping file described above 
-cs allows you to provide the cs (spherical aberration) value for your instrument (optional)
-gain_flip_rotate allows to provide instructions on gainreference flipping if needed (optional)


If you want to use it inside of another go application you can also just import it as a module using 
```sh
import github.com/oscem/Converter
```

## Life sciences mapping table: conversions.csv
This table is the heart of the actual conversion from instrument metadata output to OSCEM conform schema. Currently, it maps OSCEM fields to the outputs of EPU (xml), SerialEM (mdoc) and Tomo5 (both mdocs and xmls). Additionally it assigns units to fields and has a list of conversions to reach the targeted unit from the original output (**Units need updates if the instrument softwares change the way they output metadata - none of them explicity specifiy the units of their fields**). This also means that in order to use the table on a new or extended schema you also need to update the mapping table to match the new schema (additions only - unused mappings are irrelevant but might be useful for a different schema). Lastly it maps (parts) of the OSCEM schema onto the PDB/EMDB mmcif dictionary. This is required for the oscem-to-mmcif-converter (https://github.com/osc-em/converter-OSCEM-to-mmCIF). The nesting of the OSCEM schema is described as . seperated in the first column of the table. When modifiyng the table it is crucial to save it in UTF-8 compatible format - otherwise some of the unitnames will fail.