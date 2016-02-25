All binaries are little endian.

{int8} {int32} {int64} - per se.

{string} - {int32} = length in bytes, data bytes follow

{reladdr} - {int32} = address in bytecode relative to the start of address field

{class_def} - {class_rec}....

{class_rec} - "phfr:" literal, {int8} record type, {int64} record length, record data bytes according to length field



{method_def} - {string} = method name