# Differences between sbix and CBDT/CBLC

- `sbix` stores all of it's data (including strikes) in one table.
- `sbix` relies on standard metrics information, `CBDT/CBLC` has their own source of metrics information at a scale different to FUnits. How they differs or can conflict with the normal metric information locations is unknown to me right now.
- `sbix` can store different formats - JPEG, PDF, PNG and TIFF. `CBDT/CBLC` only does PNGs.
- `sbix` has a lot more platform support than `CBDT/CBLC`, but it's the only format Android and Google platforms support (at the time of writing).
- `sbix` does not define colour depth for the images, `CBDT/CBLC does`.